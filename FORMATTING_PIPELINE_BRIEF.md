# BRIEF: Text Formatting Pipeline for Handy

> **RUN COMMAND — paste this to Claude Code first, then this file is the spec:**
>
> ```
> claude "Read FORMATTING_PIPELINE_BRIEF.md and implement the feature end-to-end, phase by phase, exactly as specified. Do not open a PR. Run lint, fmt, and the listed checks before reporting done."
> ```
>
> Or interactively: launch `claude`, then say: `implement FORMATTING_PIPELINE_BRIEF.md`.

---

## 0. Ground rules for the implementer (read before coding)

1. **Repo governance.** Handy is under a **feature freeze** upstream (see `AGENTS.md` and `.github/PULL_REQUEST_TEMPLATE.md`). Implement this on a feature branch (`feat/formatting-pipeline`), commit cleanly, and **do not open a PR or issue**. If the user later wants it upstream, community support must be gathered in Discussions first.
2. **Conventional commits** (`feat:`, `fix:`, `refactor:`), focused on the why.
3. **i18n is mandatory.** No hardcoded strings in JSX — ESLint rejects it. Add keys to `src/i18n/locales/en/translation.json` only; other locales are handled by translation contributors.
4. **Error policy:** every formatting stage is **fail-open** — on any error (missing model, inference failure, panic) it logs `warn!` and returns the input text unchanged. This matches the existing `fail_open_text_transform` convention in `managers/transcription.rs`. Never block a paste on formatting.
5. **Formatting runs inside `process_transcription_output` BEFORE the LLM post-process step** (`post_process_transcription`, actions.rs:121). Rationale: (a) deterministic, cheap, local inference should shape text before the expensive optional LLM call sees it; (b) if post-processing is disabled, formatting still applies. Do not make the order configurable in v1.
6. **TypeScript bindings are generated.** After adding Tauri commands or `AppSettings` fields, `src/bindings.ts` regenerates via tauri-specta during `cargo build`/`tauri dev` (see `lib.rs:650-782`). Re-run the dev/build step before expecting typed access from `commands.*` on the frontend.
7. **`#[serde(default)]` on every new settings field** so existing stores deserialize without migration. Only bump `CURRENT_SETTINGS_SCHEMA_VERSION` (settings.rs) if you must rewrite existing values — additive fields don't need it.

---

## 1. Feature summary (user-facing)

A new **Text Formatting pipeline** runs on the transcript after ASR-side normalization, before LLM post-processing and before the final paste. It is a single function call containing three independently toggleable stages:

1. **Sentence segmentation** — split the flat transcript into sentence boundaries. Candidate engines (all local): `rpunct`-style ONNX models, Deep Multilingual Punctuation (HF), Silero text-enhancement models. (See §4.1 — the roster in the user's description mixes segmentation and punctuation models; treat these as candidate engines for the segmentation stage and confirm fit at implementation time.)
2. **Punctuation restoration** — restore `.,?!…` etc. User named **spaCy + downloadable language packages**; see §4.2 — spaCy is Python and is NOT a viable in-process engine for a Tauri/Rust binary. The default v1 engine must be ONNX via the `ort` runtime already in the dep tree (transcribe-rs uses it). spaCy is specced as an optional **external-process provider** (Phase 5 stretch) for users with Python installed.
3. **Advanced formatting** — TBD by design: truecasing, paragraph breaks, list/number formatting. Implement as a stub stage behind the trait so it ships with the pipeline architecture complete.

**Settings requirements:**
- Global on/off: `formatting_enabled`
- Per-step on/off: three toggles
- Per-step **model picker + downloader** with **visible language coverage** per model (reuse `supported_languages` pattern from `ModelInfo`, model.rs:61-81)

---

## 2. Where it hooks (verified against this codebase)

Current output path (`src-tauri/src/actions.rs`):

```
transcription_result (raw text from TranscriptionManager; already has
  chinese-script conversion → custom words → filler removal → normalize,
  managers/transcription.rs:1800-1830)
  → process_transcription_output(app, transcription, post_process)   [actions.rs:353]
      → post_process_transcription(settings, text)                   [actions.rs:121, optional LLM]
  → history save (hm.save_entry, actions.rs:725)
  → utils::paste(final_text) on main thread                          [actions.rs:749]
```

**Insertion point:** inside `process_transcription_output`, immediately after `let settings = get_settings(app)` (actions.rs:358) and before the `if post_process` block. The stage takes `final_text` in, returns a (possibly unchanged) string.

Also covers the headless path automatically — `run_headless_transcription` (lib.rs:435) shares the same output function; verify with a test/inspection that it does.

`ProcessedTranscription` (actions.rs:346-351) gains no new field in v1 — formatted text folds into `final_text`. (Optional follow-up: record `formatted_text` in history; requires rusqlite schema change — OUT OF SCOPE.)

Cancellation: `process_transcription_output` is wrapped in `complete_unless_cancelled` (actions.rs:709), which polls cancellation. Formatting stages are synchronous CPU work — keep each stage's per-call latency low (<500ms worst case) and check `rm.was_cancelled_since(...)` is unnecessary inside the stage; the existing wrapper suffices.

---

## 3. Settings schema (backend)

In `src-tauri/src/settings.rs`, on `AppSettings` (struct at settings.rs:376):

```rust
// Text formatting pipeline
#[serde(default)]
pub formatting_enabled: bool,
#[serde(default)]
pub formatting_segmentation_enabled: bool,
#[serde(default)]
pub formatting_punctuation_enabled: bool,
#[serde(default)]
pub formatting_advanced_enabled: bool,
/// Selected engine/model id per stage; "" = none selected (stage no-ops).
#[serde(default)]
pub formatting_segmentation_model_id: String,
#[serde(default)]
pub formatting_punctuation_model_id: String,
#[serde(default)]
pub formatting_advanced_model_id: String,
```

- No new enums strictly required — model ids are plain strings resolved against the formatting catalog. Add a `FormattingStage` enum only if it simplifies signatures:
  ```rust
  #[derive(Debug, Clone, Copy, PartialEq, Eq, Serialize, Deserialize, Type)]
  pub enum FormattingStage { Segmentation, Punctuation, Advanced }
  ```
  (`Type` = specta derive used throughout settings.rs so it exports to `bindings.ts`.)
- Defaults: `formatting_enabled` → `false` (opt-in feature, like `post_process_enabled`); each stage toggle → `true` so enabling globally enables all stages that have a selected model; model ids → `""`.

---

## 4. Backend modules

Create `src-tauri/src/formatting/` and register `mod formatting;` in `lib.rs` (alphabetical, after `mod clipboard`).

### 4.1 `formatting/mod.rs` — the orchestrator

Single entry point called from `actions.rs`:

```rust
/// Runs enabled formatting stages in fixed order:
/// segmentation → punctuation → advanced. Fail-open per stage.
pub fn run_formatting_pipeline(
    text: &str,
    settings: &AppSettings,
    models_dir: &Path,          // formatting models root, see §4.4
    language_hint: Option<&str> // effective output language, e.g. settings.selected_language
                                // ("auto" → pass None or detected lang if available)
) -> String
```

Rules inside:
- Return early if `!settings.formatting_enabled` or text is blank (reuse `is_blank_transcription` logic or move it to a shared spot).
- For each stage: `enabled && model_id resolves in catalog && model files exist on disk && model covers language_hint` → run; else `debug!`-log why skipped and pass text through.
- **Language gate is per stage:** `stage_model.supported_languages` contains the hint, or model declares `"auto"/"multilingual"`. On mismatch: skip + `warn!`.
- Wrap each stage call in `std::panic::catch_unwind` (AssertUnwindSafe) → on panic, `error!` + passthrough.

### 4.2 `formatting/engine.rs` — the stage trait

```rust
pub trait FormattingEngine: Send + Sync {
    fn stage(&self) -> FormattingStage;
    fn id(&self) -> &str;                     // catalog id
    fn supported_languages(&self) -> &[String];
    /// Cold-load model files into an ort::Session or engine state.
    fn load(&self, model_dir: &Path) -> Result<Box<dyn LoadedStage>, anyhow::Error>;
}

pub trait LoadedStage: Send {
    /// Pure transform. Implementations must not panic; catch internally.
    fn apply(&mut self, text: &str, language: Option<&str>) -> Result<String, anyhow::Error>;
}
```

Engine instances are loaded lazily and cached: `OnceCell<Mutex<HashMap<String, Arc<LoadedStage>>>>` in the formatting module (mirroring how TranscriptionManager guards its engine — see `LoadingGuard` in managers/transcription.rs for the concurrency pattern to copy: load under a guard, apply outside the lock).

### 4.3 Stage crates

- `formatting/segmentation.rs` — v1 engine: ONNX sentence-boundary model via `ort` (confirm `ort` is directly in `Cargo.toml`; if it only arrives transitively via transcribe-rs, add it explicitly). Candidate HF models to evaluate before committing — **this decision is left to the implementer, record findings in `formatting/NOTES.md`**: `oliverguhr/deepmultilingualpunctuation` (multilingual, restores punctuation+casing, usable for boundary detection), rpunct-family models, Silero text-enhancement checkpoints. Whatever ships must: accept raw ASR text, emit boundaries without dropping/reordering tokens, carry a license allowing bundled redistribution, and list `supported_languages`.
- `formatting/punctuation.rs` — **do not embed CPython/spaCy.** v1 default: an ONNX-exported punctuation-restoration model (deepmultilingualpunctuation ONNX export, or `kredor/punctuate`-family). Ship the spaCy path only as the Phase-5 optional provider below.
- `formatting/advanced.rs` — **stub implementation**: `AdvancedFormattingEngine` whose `apply` returns `Ok(text.to_string())` plus a `debug!` line. The stage toggle, model slot, and UI exist so the architecture is complete; real heuristics (truecase, paragraphs) land later.

### 4.4 `formatting/catalog.rs` — the model catalog

Mirror `src-tauri/src/catalog/mod.rs` conventions (CatalogModel → ModelDescriptor, embedded JSON, `mirror_fallbacks`, `file_in_catalog`, tests asserting unique ids / normalized scores / known languages).

New embedded file `src-tauri/src/formatting/catalog.json`; entries:

```json
{
  "id": "dmp-multilingual-onnx",
  "name": "Deep Multilingual Punctuation (ONNX)",
  "stage": "punctuation",
  "filename": "model.onnx",
  "url": "https://huggingface.co/<repo>/resolve/main/model.onnx",
  "mirrors": [],
  "sha256": "<fill>",
  "size_bytes": 0,
  "supported_languages": ["en","de","fr","es","it","nl","pl","pt", "..."],
  "tokenizer_files": [{"filename":"tokenizer.json","url":"...","sha256":"<fill>","size_bytes":0}],
  "description": "Restores punctuation and casing for ~30 languages."
}
```

Download destination: **keep formatting models separate from ASR models** — `{app_data_dir}/formatting_models/<model_id>/` (use `crate::portable`-aware path helper; see how `models` dir is resolved in `managers/model.rs` and `portable.rs`).

### 4.5 `formatting/manager.rs` — download/delete/status

Reuse the `hf_hub` tokio API + `CancellationToken` + progress-emitting pattern already in `managers/model.rs` (lines ~489-700 show the download-state guard, `is_downloading` bookkeeping, partial-size tracking). Do **not** fork it wholesale — extract the smallest possible copy: single-file download with sha256 verify + progress events (`formatting-model-download-progress`, `{model_id, downloaded, total}`) + cancel + delete. If the existing downloader can be generalized cheaply (it already handles arbitrary files + mirrors), prefer that — decide in code, document the choice.

---

## 5. Tauri commands

New file `src-tauri/src/commands/formatting.rs`, registered next to the other command modules; each command `#[tauri::command] #[specta::specta]`:

| Command | Purpose |
|---|---|
| `get_formatting_models(stage: FormattingStage) -> Vec<FormattingModelInfo>` | catalog entries + `is_downloaded`, `is_downloading`, `partial_size`, `supported_languages` |
| `download_formatting_model(model_id: String)` | async download w/ progress events |
| `cancel_formatting_download(model_id: String)` | |
| `delete_formatting_model(model_id: String)` | |
| `get_formatting_model_status() -> Vec<...>` | cheap status poll while page open (mirror `get_transcription_model_status`) |

Register in the `collect_commands(...)` list and `.invoke_handler(...)` in `lib.rs:650+`. Add `mod formatting;` under `commands/`.

---

## 6. Frontend

### 6.1 Sidebar entry

`src/components/Sidebar.tsx` (~line 50): add a `formatting` entry between `models` and `advanced` (or after `postprocessing` — pick the position that reads naturally; icons come from `lucide-react`, e.g. `Pilcrow` or `TextQuote`):

```ts
formatting: {
  labelKey: "sidebar.formatting",
  icon: Pilcrow,
  component: FormattingSettings,
  enabled: () => true,           // always visible — the page contains the global toggle
},
```

### 6.2 Components (new dir `src/components/settings/formatting/`)

- `FormattingSettings.tsx` — page: global `FormattingToggle` + three `FormattingStageCard`s.
- `FormattingToggle.tsx` — bound to `settings.formatting_enabled` via `useSettings().updateSetting` (store at `src/stores/settingsStore.ts`, uses `commands.updateSettings`-style path — follow exactly how `PostProcessingToggle.tsx`/`FillerWordRemoval.tsx` call `updateSetting`; both exist in `src/components/settings/`).
- `FormattingStageCard.tsx` — per stage: title + description + toggle (`formatting_<stage>_enabled`) + `FormattingModelPicker`.
- `FormattingModelPicker.tsx` — modeled on `src/components/model-selector/ModelSelector.tsx` + `ModelDropdown.tsx` + `DownloadProgressDisplay.tsx`: dropdown of catalog models for the stage (disabled state shows "not downloaded"), download button with progress, delete button, and — requirement — **language coverage**: render `supported_languages` as chips or a `<details>` list ("Covers 27 languages: en, de, …"). If `settings.selected_language !== 'auto'` and the selected model doesn't cover it, show an inline warning.
- `index.ts` barrel export; add `export { FormattingSettings } from "./formatting/FormattingSettings";` in `src/components/settings/index.ts`.

### 6.3 i18n keys (en/translation.json)

```
"sidebar": { "formatting": "Text Formatting" },
"settings": { "formatting": {
  "title": "Text Formatting",
  "description": "...",
  "enabled": "...",
  "segmentation": { "title", "description" },
  "punctuation": { ... },
  "advanced": { ... , "note": "Coming soon — stub stage" },
  "languagesCovered": "Covers {{count}} languages",
  "languageMismatch": "This model does not support your transcription language ({{lang}})"
}}
```

---

## 7. Execution phases (do them in this order)

- **Phase 1 — settings + schema.** Add fields + defaults + `FormattingStage` enum; `cargo check`; confirm `bindings.ts` regen picks up fields.
- **Phase 2 — pipeline skeleton.** `formatting/` module with orchestrator, trait, three stage stubs (all passthrough), wired into `process_transcription_output`. Unit tests: disabled → unchanged; stage off → skipped; panic in stage → passthrough; language mismatch → skipped. `cargo test`.
- **Phase 3 — catalog + downloads.** `catalog.json` (start with ONE punctuation ONNX model you have verified; empty-stage catalogs return empty lists), manager, commands. `cargo clippy` clean.
- **Phase 4 — frontend.** Sidebar entry, page, cards, picker, i18n. `bun run lint`, `bun run build`.
- **Phase 5 — real engines.** Swap segmentation/punctuation stubs for ONNX implementations (record engine evaluation in `formatting/NOTES.md`). OPTIONAL stretch, behind a per-user opt-in flag only if implemented at all: spaCy via subprocess (`python3 -m spacy ...`) when a system Python + spaCy are detected — must degrade silently when absent.
- **Phase 6 — verify end-to-end** (see §8) + `bun run format` + final report.

## 8. Acceptance checks

1. `cargo test` — new unit tests pass.
2. `bun run lint` — zero errors (i18n enforcement included).
3. `cargo fmt` + `bun run format:frontend` — clean.
4. Manual: `bun run tauri dev` → Settings → Text Formatting → enable → download a punctuation model → dictate unpunctuated speech → pasted text is punctuated → toggle stage off → verbatim text pastes again → set `selected_language` to an uncovered language → stage skips with warning in logs.
5. Headless parity: `run_headless_transcription` output also formatted (verify path).
6. No PR opened. Branch `feat/formatting-pipeline`, conventional commits per phase.

## 9. Key file map (verified)

| File | Why |
|---|---|
| `src-tauri/src/actions.rs:353` `process_transcription_output` | insertion point |
| `src-tauri/src/actions.rs:121` `post_process_transcription` | ordering sibling |
| `src-tauri/src/settings.rs:376` `AppSettings` | new fields |
| `src-tauri/src/commands/models.rs` | command pattern to mirror |
| `src-tauri/src/commands/mod.rs:42` `get_app_settings` | settings flow to frontend |
| `src-tauri/src/catalog/mod.rs` | catalog pattern |
| `src-tauri/src/managers/model.rs` | hf_hub download + progress + cancel pattern |
| `src-tauri/src/audio_toolkit/text.rs` | existing pure text transforms (normalize, fillers) — style reference |
| `src-tauri/src/lib.rs:650-782` | specta command registration + bindings export |
| `src/components/Sidebar.tsx:50-65` | nav registration + `enabled` gating |
| `src/components/settings/post-processing/` | page pattern to mirror |
| `src/components/model-selector/` | picker/download UI to reuse |
| `src/stores/settingsStore.ts:316` `updateSetting` | settings write path |
| `src/hooks/useSettings.ts` | settings read hook |
| `src/i18n/locales/en/translation.json` | all new strings here |

## 10. Explicit non-goals (v1)

- No history-schema change (formatted text not separately recorded).
- No spaCy embedding; external-provider path is stretch-only.
- No ordering config vs. LLM post-processing.
- No cloud formatting providers.
- No streaming-time formatting — final text only (the live preview path uses `normalize_transcription_output` separately; leave untouched).
