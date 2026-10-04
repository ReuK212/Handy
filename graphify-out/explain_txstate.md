# Explain: `TxState` — the shared protocol behind reliable paste

_Source: graph traversal of `graphify-out/graph.json` + inspection of `src-tauri/src/paste_tx/mod.rs`. Generated from the question "why does `TxState` bridge Windows Paste, macOS Paste, and other communities?" — betweenness centrality 0.021, third-highest in the graph._

## What it is

`TxState` (`src-tauri/src/paste_tx/mod.rs:67`) is a **cross-platform transaction record for receipt-sequenced clipboard paste** — Handy's "reliable paste" path (debug-gated). It exists because the legacy paste had a race condition:

- Legacy flow (`clipboard::paste_via_clipboard`): put transcript on clipboard → inject Ctrl+V keystroke → restore old clipboard after a *fixed delay*. But the keystroke is only *enqueued* — the target app reads the clipboard whenever its event loop gets to it. On a slow or busy machine the restore could fire before the read, and the user gets their **old** clipboard pasted back (issue #502).
- Reliable flow: publish the transcript as a **lazy promise** (Windows `SetClipboardData(CF_UNICODETEXT, NULL)` delayed rendering; macOS `declareTypes:owner:`) → inject the paste chord → wait for the OS to report that a consumer actually *read* the clipboard (a "receipt": `WM_RENDERFORMAT` on Windows, `pasteboard:provideDataForType:` on macOS) → only then restore.

`TxState` is the state shared between the two platform event loops: `published_at`, `injected_at`, `injection_failed`, `receipts`, `ownership_lost`, `cancelled`, `auto_submit_sent`, `logged_receipt`.

## Why it bridges Windows Paste ↔ macOS Paste

The graph shows `TxState` is `references`d by `ProviderIvars` and `MacPending` (community 32, macOS Paste) and `WinTxShared` (community 20, Windows Paste). Same struct, two OS event loops — because the *protocol* is identical even though the receipt mechanisms differ:

1. Only receipts **after `injected_at`** count (a read before the chord is an eager third party — clipboard manager, antivirus — not the target).
2. Restore only while we still **own** the clipboard (sequence/changeCount unchanged; user copies win).
3. Restore after a **200ms quiet period** following the last receipt (Chromium probes then re-reads).
4. Bounded by timeouts: 8s `RESTORE_TIMEOUT`, 500ms `FAILED_INJECTION_TIMEOUT` — failure mode is always "transcript stays longer", never "stale content pasted".

The pure `evaluate(&TxState, now) -> WaitDecision` (mod.rs:139) is the shared verdict both platform loops call — which is why `TxState` also `references` `evaluate()` and the test helper `state_after_publish()`. Communities bridged: Windows Paste (20), macOS Paste (32), and the mod.rs test cluster (72/94).

## Sibling finding: `get_default_settings()` (god node #6, 53 edges)

Different hub shape from `get_settings()`: it has **51 inbound + 33 outbound** edges — it *fans out*. It's the factory that composes ~30 `default_*()` helpers (`default_paste_delay_ms`, `default_vad_enabled`, `default_overlay_style`, `default_typing_tool`, `default_post_process_providers`, …) into a full `AppSettings`, and its biggest inbound cluster is `settings.rs`'s own **migration and salvage tests** (~15 test fns in the Settings Backend community) plus `get_settings()` and `salvage_settings()` themselves. In short: `get_default_settings` is the *schema's source of truth* — every migration test asserts "old store + defaults = correct new store", which is why the test suite surrounds it.

## Related

`paste()` (clipboard.rs:774) — the fallback legacy path `try_reliable_paste` returns `Err` into · `AutoSubmitKey`/`ClipboardHandling`/`PasteMethod` — the settings enums both paths consume · `send_chord()` — the shared keystroke injector (100ms `CHORD_HOLD_MS`, kept at parity with legacy per #165)
