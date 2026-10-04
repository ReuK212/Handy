# Explain: `get_settings()` — Handy's settings funnel

_Source: graph traversal of `graphify-out/graph.json` (3,211 nodes / 7,479 edges) + code inspection. Generated from the question "why is `get_settings()` a bridge across 16 communities?" — it had the highest cross-community betweenness centrality (0.027) of any non-library node in the graph._

## What it is

`get_settings(app: &AppHandle) -> AppSettings` at `src-tauri/src/settings.rs:1037` is **the canonical read accessor for all persisted application settings**. Nothing in Handy reads the settings store directly — every subsystem funnels through this one function. It is the single synchronization point between the on-disk `settings` JSON (tauri-plugin-store, via `crate::portable::store_path`) and live code.

It is not a plain getter. On every call it can also **write**:

1. **Salvage** — if the stored `AppSettings` fails `serde_json` deserialization, `salvage_settings()` (settings.rs:1090, added for issue #1619) rebuilds it field-by-field so one bad value can't reset the user's whole config.
2. **Migrate** — `apply_settings_migrations()` upgrades older schema versions in place (idempotent, converges after first read).
3. **Backfill bindings** — new default keyboard bindings added since the store was written are merged into `settings.bindings`.
4. **Post-process defaults** — `ensure_post_process_defaults()` normalizes LLM post-processing config.

So `get_settings()` is really a *read-repair* function: every read silently heals and upgrades the store. That's the first reason it's central — correctness depends on it being called, not just convenience.

## Who calls it — 50 `calls` edges across 14 files / 16 communities

| Subsystem | File | Calls | What it reads |
|---|---|---|---|
| Audio commands | `commands/audio.rs` | 9 | `selected_microphone`, `selected_output_device`, `selected_channel`, `clamshell` mic, mic mode |
| Recording lifecycle | `managers/audio.rs` | 9 | `always_on_microphone`, `vad_backend`, `mute_while_recording`, `selected_channel`; also writes back (`persist_default_microphone_after_fallback` clears a dead mic) |
| Transcription engine | `managers/transcription.rs` | 8 | accelerator/device settings, model unload timeout, streaming/finalize behavior, language, x64-emulation CPU override |
| Settings internals | `settings.rs` | 4 | thin wrappers: `get_bindings()`, `get_history_limit()`, `get_recording_retention_period()`, `load_or_create_app_settings()` |
| Shortcuts | `shortcut/mod.rs`, `handy_keys.rs`, `handler.rs` | 7 | `bindings` map — which hotkeys to register and how to interpret them |
| Pipeline actions | `actions.rs` | 3 | `selected_model`, `always_on_microphone`, `overlay_style`, post-processing config |
| Model commands | `commands/models.rs` | 3 | `selected_model` for get/switch/delete |
| Misc commands | `commands/mod.rs`, `commands/transcription.rs` | 3 | `get_app_settings` (the frontend-facing Tauri command), `set_log_level`, model unload timeout |
| Output | `clipboard.rs` | 1 | `paste_method`, `paste_delay_ms`, `paste_delay_after_ms`, `append_trailing_space`, clipboard handling, auto-submit key, typing tool |
| Startup | `lib.rs` | 2 | `run()` and `run_headless_transcription()` bootstrap |
| Model manager | `managers/model.rs` | 1 | `auto_select_model_if_needed` |

Community IDs touched: 0 (Shortcuts & Settings), 3 (Clipboard & Paste), 13 (Hotkey Handling), 14 (Audio Recording Manager), 17 (Recording Overlay), 21 (App Initialization), 22 (Transcription Actions), 24 (Audio Commands), 29 (Transcription Engine Loading), 33 (Streaming Transcription), 36 (Model Manager), 49/50/54/84/85 (command/UI fragments). Its neighbors also include `AppSettings` (the type it returns) and `AppHandle` (the Tauri state it needs).

## Why it shows up as a cross-community bridge

- **Every subsystem reads config.** Communities are detected by edge density; subsystems cluster internally, and `get_settings()` is one of the few nodes with edges into *all* of them. Betweenness centrality 0.027 means a disproportionate share of shortest paths between communities route through it.
- **Fan-in, not fan-out.** It returns the whole `AppSettings` snapshot; each caller picks its own fields. Handy deliberately has no per-field getters, so the hub is maximal — 50 callers rather than 5 accessor nodes each with 10.
- **Its sibling `get_default_settings()` is god node #6** (53 edges). `get_settings` itself calls it for the defaults merge — the defaults are the fallback behind every field, so both nodes sit at the hub.
- **Frontend parity.** `get_app_settings` (commands/mod.rs:42) exposes it to the React side over the Tauri bridge, mirroring `useSettings()` — the #2 god node on the TypeScript side. The graph shows the same hub pattern in both languages: one Rust accessor, one Zustand hook.

## Consequences worth knowing

- **Read = potential write.** Any call site can trigger migration/salvage writes. That's usually invisible, but it means `get_settings()` is not side-effect-free — in a hot loop it still round-trips the JSON store. (Recording starts in `actions.rs` read it once per recording, not per frame — that's fine.)
- **Snapshot semantics.** Callers get a point-in-time copy; there's no subscription in Rust. The store→frontend sync is via `settings-changed` events after `write_settings()`. Two concurrent `get_settings` → mutate → `write_settings` sequences can lose updates — the graph shows both `managers/audio.rs` (mic fallback) and `commands/mod.rs` (log level) doing read-modify-write.
- **Testing leverage point.** Because it's the only funnel, stubbing `get_settings` covers config for the whole backend — which is why `settings.rs`'s own wrappers and tests cluster around it.

## Trace path (how the answer was found)

`suggest_questions` flagged `get_settings()` for high betweenness → `graphify query` BFS depth-2 returned 628 reachable nodes → direct edge inspection of `graph.json` (`calls` edges with `target = src_tauri_src_settings_get_settings`) enumerated the 50 callers above → `settings.rs:1037-1083` confirmed the read-repair behavior.

## Related god nodes

`useSettings()` (112 edges, frontend twin) · `react` (112) · `get_default_settings()` (53) · `AppSettings` (40) · `write_settings()` (paired mutator)
