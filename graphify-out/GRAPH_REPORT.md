# Graph Report - Handy  (2026-10-04)

## Corpus Check
- 363 files · ~317,730 words
- Verdict: corpus is large enough that graph structure adds value.
- Unclassified: 24 file(s) not represented in the graph (top: (none) 6, .nix 4, .css 3)

## Summary
- 3211 nodes · 7479 edges · 140 communities (120 shown, 20 thin omitted)
- Extraction: 97% EXTRACTED · 3% INFERRED · 0% AMBIGUOUS · INFERRED: 210 edges (avg confidence: 0.86)
- Token cost: 2,063,963 input · 104,528 output

## Community Hubs (Navigation)
- Shortcuts & Settings
- Transcription Coordinator
- Settings UI Components
- Clipboard & Paste
- Model Selector UI
- Tauri Bindings
- Text Post-Processing
- Settings Backend
- App Shell & Onboarding
- System Tray
- History Storage
- LLM Client
- Utility Scripts
- Hotkey Handling
- Audio Recording Manager
- Settings UI (Misc)
- Platform Helpers
- Recording Overlay
- Debug & About UI
- Footer & Misc UI
- Windows Paste
- App Initialization
- Transcription Actions
- macOS Input
- Audio Commands
- Transcription Manager
- Model Download Tests
- Debug UI Components
- Frontend Tooling Config
- Transcription Engine Loading
- Audio Device Toolkit
- Tauri Bundle Config
- macOS Paste
- Streaming Transcription
- Model Catalog
- Model Capability Probe
- Model Manager
- Model Discovery
- Model Downloads
- Audio Resampler
- Portable Mode
- NPM Dependencies
- Audio Recorder
- CI Workflows
- Audio Capture
- Community 45
- Community 46
- Community 47
- Community 48
- Community 49
- Community 50
- Community 51
- Community 52
- Community 53
- Community 54
- Community 55
- Community 56
- Community 57
- Community 58
- Community 59
- Community 60
- Community 61
- Community 62
- Community 63
- Community 64
- Community 65
- Community 66
- Community 67
- Community 68
- Community 69
- Community 70
- Community 71
- Community 72
- Community 73
- Community 74
- Community 75
- Community 76
- Community 77
- Community 78
- Community 79
- Community 80
- Community 81
- Community 82
- Community 83
- Community 84
- Community 85
- Community 86
- Community 87
- Community 88
- Community 89
- Community 90
- Community 91
- Community 92
- Community 93
- Community 94
- Community 95
- Community 96
- Community 97
- Community 98
- Community 99
- Community 100
- Community 101
- Community 102
- Community 103
- Community 104
- Community 105
- Community 106
- Community 107
- Community 108
- Community 109
- Community 110
- Community 111
- Community 112
- Community 113
- Community 114
- Community 115
- Community 116
- Community 117
- Community 118
- Community 119
- Community 120
- Community 122
- Community 123
- Community 124
- Community 125
- Community 126
- Community 127
- Community 128
- Community 129
- Community 130
- Community 131
- Community 132
- Community 133
- Community 134
- Community 135

## God Nodes (most connected - your core abstractions)
1. `react` - 112 edges
2. `useSettings()` - 112 edges
3. `react-i18next` - 76 edges
4. `SettingContainer()` - 76 edges
5. `get_settings()` - 57 edges
6. `get_default_settings()` - 53 edges
7. `TranscriptionManager` - 49 edges
8. `Dropdown()` - 46 edges
9. `AppSettings` - 40 edges
10. `AudioRecordingManager` - 38 edges

## Surprising Connections (you probably didn't know these)
- `CLI flags (--toggle-transcription, --toggle-post-process, --cancel, --start-hidden, --no-tray, --debug)` --semantically_similar_to--> `CLI parameters - remote control and startup flags`  [INFERRED] [semantically similar]
  AGENTS.md → README.md
- `ydotool typing fix for Wayland (systemd user service)` --semantically_similar_to--> `Linux text input tools - xdotool (X11), wtype (Wayland), dotool, ydotool (Ubuntu 26.04)`  [INFERRED] [semantically similar]
  docs/troubleshooting/ubuntu-26-04-gnome-wayland/README.md → README.md
- `Silero VAD (vad-rs) - voice activity detection filtering` --semantically_similar_to--> `Silero VAD silence filtering`  [INFERRED] [semantically similar]
  AGENTS.md → README.md
- `i18next internationalization system (src/i18n/locales, en translation.json source)` --semantically_similar_to--> `src/i18n/locales/<lang>/translation.json per-language structure (ISO 639-1 codes)`  [INFERRED] [semantically similar]
  AGENTS.md → CONTRIBUTING_TRANSLATIONS.md
- `Code style guidelines (cargo fmt/clippy, strict TS no any, functional components, Tailwind)` --semantically_similar_to--> `Rust style - anyhow::Error, Arc<Mutex<T>> shared state, builder pattern, log levels`  [INFERRED] [semantically similar]
  CONTRIBUTING.md → CRUSH.md

## Import Cycles
- None detected.

## Hyperedges (group relationships)
- **Seven-platform build matrix dispatching to reusable Build workflow (macOS ARM/Intel, Ubuntu 22.04/24.04/24.04-ARM, Windows x64/ARM64)** — _github_workflows_build_test_job_build_test, _github_workflows_main_build_job_build, _github_workflows_pr_test_build_job_build_test, _github_workflows_release_job_publish_tauri, _github_workflows_build_workflow [INFERRED 0.85]
- **Release pipeline: draft release creation -> matrix build -> signed asset upload** — _github_workflows_release_job_create_release, _github_workflows_release_job_publish_tauri, _github_workflows_build_workflow, _github_workflows_release_concept_draft_release [INFERRED 0.95]
- **Path-filtered CI checks gating pull requests (quality, nix, playwright, rust tests)** — _github_workflows_code_quality_workflow, _github_workflows_nix_check_workflow, _github_workflows_playwright_workflow, _github_workflows_test_workflow [INFERRED 0.75]
- **Manager Pattern organizing core business logic** — agents_manager_pattern, agents_audio_manager, agents_model_manager, agents_transcription_manager, agents_history_manager [EXTRACTED 1.00]
- **Audio to VAD to speech model to text to clipboard/paste pipeline** — agents_audio_toolkit, agents_silero_vad, agents_transcribe_cpp, agents_transcribe_rs, agents_transcription_manager [EXTRACTED 1.00]
- **Single-instance CLI remote control mechanism** — agents_cli_flags, agents_single_instance, agents_send_transcription_input, readme_cli_flags [INFERRED 0.95]
- **Android ic_launcher icon across densities** — src_tauri_icons_android_mipmap_hdpi_ic_launcher, src_tauri_icons_android_mipmap_mdpi_ic_launcher, src_tauri_icons_android_mipmap_xhdpi_ic_launcher, src_tauri_icons_android_mipmap_xxhdpi_ic_launcher, src_tauri_icons_android_mipmap_xxxhdpi_ic_launcher [INFERRED 0.95]
- **Android ic_launcher_foreground icon across densities** — src_tauri_icons_android_mipmap_hdpi_ic_launcher_foreground, src_tauri_icons_android_mipmap_mdpi_ic_launcher_foreground, src_tauri_icons_android_mipmap_xhdpi_ic_launcher_foreground, src_tauri_icons_android_mipmap_xxhdpi_ic_launcher_foreground, src_tauri_icons_android_mipmap_xxxhdpi_ic_launcher_foreground [INFERRED 0.95]
- **Android ic_launcher_round icon across densities** — src_tauri_icons_android_mipmap_hdpi_ic_launcher_round, src_tauri_icons_android_mipmap_mdpi_ic_launcher_round, src_tauri_icons_android_mipmap_xhdpi_ic_launcher_round, src_tauri_icons_android_mipmap_xxhdpi_ic_launcher_round, src_tauri_icons_android_mipmap_xxxhdpi_ic_launcher_round [INFERRED 0.95]
- **iOS App Icon asset set (resamples of Handy microphone icon)** — src_tauri_icons_ios_appicon_20x20_1x, src_tauri_icons_ios_appicon_20x20_2x_1, src_tauri_icons_ios_appicon_20x20_2x, src_tauri_icons_ios_appicon_20x20_3x, src_tauri_icons_ios_appicon_29x29_1x, src_tauri_icons_ios_appicon_29x29_2x_1, src_tauri_icons_ios_appicon_29x29_2x, src_tauri_icons_ios_appicon_29x29_3x, src_tauri_icons_ios_appicon_40x40_1x, src_tauri_icons_ios_appicon_40x40_2x_1, src_tauri_icons_ios_appicon_40x40_2x, src_tauri_icons_ios_appicon_40x40_3x, src_tauri_icons_ios_appicon_512_2x, src_tauri_icons_ios_appicon_60x60_2x, src_tauri_icons_ios_appicon_60x60_3x, src_tauri_icons_ios_appicon_76x76_1x, src_tauri_icons_ios_appicon_76x76_2x, src_tauri_icons_ios_appicon_83_5x83_5_2x [INFERRED 0.95]
- **Tray icon state set (idle/recording/transcribing x light/dark + warning)** — src_tauri_resources_tray_idle, src_tauri_resources_tray_idle_dark, src_tauri_resources_tray_idle_warning, src_tauri_resources_tray_idle_warning_dark, src_tauri_resources_tray_recording, src_tauri_resources_tray_recording_dark, src_tauri_resources_tray_transcribing, src_tauri_resources_tray_transcribing_dark [INFERRED 0.85]
- **App status glyph set (logo, warning, recording, transcribing)** — src_tauri_resources_handy, src_tauri_resources_handy_warning, src_tauri_resources_recording, src_tauri_resources_transcribing [INFERRED 0.85]

## Communities (140 total, 20 thin omitted)

### Community 0 - "Shortcuts & Settings"
Cohesion: 0.06
Nodes (99): apple_intelligence_default_model_id, AvailableAccelerators, KeyboardImplementation, LLMPrompt, OrtAcceleratorSetting, add_post_process_prompt(), apply_window_theme(), BindingResponse (+91 more)

### Community 1 - "Transcription Coordinator"
Cohesion: 0.06
Nodes (69): autorepeat_burst(), BINDING, BusyAction, cancel_during_processing_drops_remembered_press(), classify_busy_input(), classify_ptt_event(), Command, CoordinatorState (+61 more)

### Community 2 - "Settings UI Components"
Cohesion: 0.08
Nodes (68): react-i18next, AccelerationSelector(), AdvancedSettings(), AlwaysOnMicrophone, AlwaysOnMicrophoneProps, AppendTrailingSpace, AppendTrailingSpaceProps, AudioFeedback (+60 more)

### Community 3 - "Clipboard & Paste"
Cohesion: 0.06
Nodes (67): cell, clipboard, duration, info, overlay, shortcut, classify_ydotool_key_syntax(), clipboard_is_restored_before_key_injection_error_is_returned() (+59 more)

### Community 4 - "Model Selector UI"
Cohesion: 0.05
Nodes (58): i18next, immer, @tauri-apps/plugin-dialog, zustand, CatalogModel, failures, ModelInfo, DownloadProgress (+50 more)

### Community 5 - "Tauri Bindings"
Cohesion: 0.05
Nodes (60): AppSettings, AudioDevice, AutoSubmitKey, AvailableAccelerators, BindingResponse, ChineseScript, ClipboardHandling, commands (+52 more)

### Community 6 - "Text Post-Processing"
Cohesion: 0.06
Nodes (67): levenshtein, Regex, soundex, apply_custom_words(), build_custom_word_match_keys(), build_match_key(), build_ngram(), collapse_stutters() (+59 more)

### Community 7 - "Settings Backend"
Cohesion: 0.05
Nodes (63): de, APPLE_INTELLIGENCE_DEFAULT_MODEL_ID, APPLE_INTELLIGENCE_PROVIDER_ID, apply_settings_migrations(), chinese_script_migration_only_carries_over_legacy_intents(), ChineseScript, CURRENT_SETTINGS_SCHEMA_VERSION, debug_output_redacts_api_keys() (+55 more)

### Community 8 - "App Shell & Onboarding"
Cohesion: 0.06
Nodes (50): @tauri-apps/plugin-os, tauri-plugin-macos-permissions-api, App(), OnboardingStep, renderSettingsContent(), events, StreamPhase, StreamPhaseEvent (+42 more)

### Community 9 - "System Tray"
Cohesion: 0.06
Nodes (55): chinese_script_for_locale, get_tray_translations, hashmap, history, lazy, Menu, apply_on_main(), AppTheme (+47 more)

### Community 10 - "History Storage"
Cohesion: 0.08
Nodes (41): anyhow, chrono, Connection, M, managers, PaginatedHistory, process_transcription_output, RecordingRetentionPeriod (+33 more)

### Community 11 - "LLM Client"
Cohesion: 0.08
Nodes (54): Client, error_as_stderror, fmt, header, HeaderMap, PostProcessProvider, build_headers(), ChatChoice (+46 more)

### Community 12 - "Utility Scripts"
Cohesion: 0.05
Nodes (54): argparse, boto3, boto3_s3_transfer, collections, concurrent_futures, datetime, hashlib, huggingface_hub (+46 more)

### Community 13 - "Hotkey Handling"
Cohesion: 0.07
Nodes (43): handle_shortcut_event, HotkeyId, HotkeyManager, KeyboardListener, log, Modifiers, mpsc, handle_shortcut_event() (+35 more)

### Community 14 - "Audio Recording Manager"
Cohesion: 0.08
Nodes (30): AudioRecordingManager, create_audio_recorder(), DesiredMicrophone, EARSHOT_VAD_THRESHOLD, get_mute(), MicrophoneMode, MicrophoneResolution, MuteState (+22 more)

### Community 15 - "Settings UI (Misc)"
Cohesion: 0.07
Nodes (39): react, react, CustomWordsProps, normalizeCustomWord(), DebugPaths(), DebugPathsProps, HistoryLimitProps, PasteMethodProps (+31 more)

### Community 16 - "Platform Helpers"
Cohesion: 0.09
Nodes (48): command, Hotkey, is_clamshell(), is_laptop(), String, test_clamshell_check(), test_is_laptop(), carbon_equivalent() (+40 more)

### Community 17 - "Recording Overlay"
Cohesion: 0.07
Nodes (43): gtk_layer_shell, input, Monitor, calculate_overlay_position(), create_recording_overlay(), current_overlay_logical_size(), emit_levels(), emit_recording_ready() (+35 more)

### Community 18 - "Debug & About UI"
Cohesion: 0.08
Nodes (34): AboutSettings(), AppDataDirectory(), AppDataDirectoryProps, DebugSettingsProps, HoldThreshold(), HoldThresholdProps, formatTime(), LEVEL_META (+26 more)

### Community 19 - "Footer & Misc UI"
Cohesion: 0.07
Nodes (32): lucide-react, ref_node_assert, @tauri-apps/api, @tauri-apps/plugin-opener, HistoryEntry, HistoryUpdatePayload, SecureInputStatus, Footer() (+24 more)

### Community 20 - "Windows Paste"
Cohesion: 0.09
Nodes (43): core, dataexchange, GDI_IMAGE_TYPE, getmodulehandlew, globalfree, HINSTANCE, HWND, instant (+35 more)

### Community 21 - "App Initialization"
Cohesion: 0.07
Nodes (37): App, atomic, AtomicU8, builder_as_envfilterbuilder, env_flag_enabled, Filter, image, LevelFilter (+29 more)

### Community 22 - "Transcription Actions"
Cohesion: 0.08
Nodes (31): apple_intelligence, C, future, Output, OverlayStyle, ACTION_MAP, build_system_prompt(), CANCELLATION_POLL_INTERVAL (+23 more)

### Community 23 - "macOS Input"
Cohesion: 0.09
Nodes (35): c_void, CfDataRef, CfStringRef, ANSI_V_KEYCODE, CFDataGetBytePtr(), CFRelease(), COMMAND_MODIFIER_STATE, command_v_key() (+27 more)

### Community 24 - "Audio Commands"
Cohesion: 0.13
Nodes (37): HKEY, AudioDevice, check_custom_sounds(), custom_sound_exists(), CustomSounds, get_available_microphones(), get_available_output_devices(), get_clamshell_microphone() (+29 more)

### Community 25 - "Transcription Manager"
Cohesion: 0.11
Nodes (32): event, panic, auto_detected_chinese_text_is_converted(), auto_language_uses_single_language_model_as_evidence(), auto_language_without_detection_skips_gated_filler_removal(), effective_language_for_model(), fail_open_text_transform(), ignored_user_language_is_not_output_evidence() (+24 more)

### Community 26 - "Model Download Tests"
Cohesion: 0.16
Nodes (34): net, http_416_short_of_expected_size_clears_partial(), http_416_with_hash_finalizes_complete_partial(), http_416_with_wrong_hash_clears_partial(), http_416_without_trust_signals_clears_partial(), http_body_exceeding_expected_size_aborts_and_clears(), http_cancel_during_stalled_body_keeps_partial(), http_cancel_while_awaiting_headers() (+26 more)

### Community 27 - "Debug UI Components"
Cohesion: 0.11
Nodes (25): sonner, KeyboardDiagnosticReport, ResetIcon(), ResetIconProps, KeyboardDiagnostic(), ReliablePasteToggle(), ReliablePasteToggleProps, GlobalShortcutInput() (+17 more)

### Community 28 - "Frontend Tooling Config"
Cohesion: 0.06
Nodes (30): name, private, type, version, eslint, eslint-plugin-i18next, ref_path, @playwright/test (+22 more)

### Community 29 - "Transcription Engine Loading"
Cohesion: 0.12
Nodes (15): Condvar, apply_accelerator_settings(), LoadingGuard, real_time_factor(), AppHandle, Arc, AtomicBool, AtomicU64 (+7 more)

### Community 30 - "Audio Device Toolkit"
Cohesion: 0.12
Nodes (25): audio_toolkit, io, CpalDeviceInfo, list_input_devices(), list_output_devices(), Box, Device, Error (+17 more)

### Community 31 - "Tauri Bundle Config"
Cohesion: 0.06
Nodes (32): bundleMediaFramework, files, bundle, active, createUpdaterArtifacts, icon, license, linux (+24 more)

### Community 32 - "macOS Paste"
Cohesion: 0.13
Nodes (29): clipboardext, enigostate, NSInteger, objc2, objc2_app_kit, objc2_foundation, Retained, send_return_key (+21 more)

### Community 33 - "Streaming Transcription"
Cohesion: 0.12
Nodes (18): Any, OutputLanguageEvidence, cpp_translation_task(), drain_until_finalize(), FinalizedStreamText, ModelStateEvent, panic_payload_message(), Option (+10 more)

### Community 34 - "Model Catalog"
Cohesion: 0.11
Nodes (22): btreeset, CatalogModel, Deserialize, known_arches, CATALOG, CatalogCaps, CatalogModel, CatalogRoot (+14 more)

### Community 35 - "Model Capability Probe"
Cohesion: 0.10
Nodes (25): serde, CapabilityProbe, CapabilityProber, Compatibility, GgufHeaderProber, KEY_ARCH, KEY_CAP_LANG_DETECT, KEY_CAP_STREAMING (+17 more)

### Community 36 - "Model Manager"
Cohesion: 0.17
Nodes (10): DownloadCleanup, hf_cached_path(), ModelInfo, ModelManager, Arc, CancellationToken, HashMap, HashSet (+2 more)

### Community 37 - "Model Discovery"
Cohesion: 0.15
Nodes (19): build_test_gguf_string_metadata(), canonicalize_supported_languages(), DownloadProgress, local_caps(), LocalCaps, probed_display_name(), push_gguf_str(), AppHandle (+11 more)

### Community 38 - "Model Downloads"
Cohesion: 0.10
Nodes (20): archive, emitter, file, fs, gzdecoder, hf_hub, sha2, base_language() (+12 more)

### Community 39 - "Audio Resampler"
Cohesion: 0.18
Nodes (20): FftFixedIn, rubato, assert_tail_burst_flushed(), collect_output(), finish_does_not_leak_tail_into_next_session(), finish_flushes_resampler_delay(), finish_flushes_resampler_delay_44100(), finish_flushes_unaligned_tail() (+12 more)

### Community 40 - "Portable Mode"
Cohesion: 0.14
Nodes (17): manager, app_data_dir(), app_log_dir(), data_dir(), hugging_face_home(), init(), is_valid_portable_marker(), PORTABLE_DATA_DIR (+9 more)

### Community 41 - "NPM Dependencies"
Cohesion: 0.08
Nodes (26): dependencies, i18next, immer, lucide-react, react-dom, react-i18next, react-markdown, react-select (+18 more)

### Community 42 - "Audio Recorder"
Cohesion: 0.20
Nodes (12): AudioRecorder, Arc, Box, Device, Error, F, JoinHandle, Mutex (+4 more)

### Community 43 - "CI Workflows"
Cohesion: 0.09
Nodes (25): Binary signing flow (Apple certificate, trusted-signing-cli/Azure, Tauri updater key), build job (Tauri cross-platform compile, sign, audit, upload), Rationale: libvulkan/libwayland-client excluded from AppImage so host graphics stack is used, Rationale: Windows x64 dynamically links Microsoft's SSE2-baseline ONNX Runtime because pyke's AVX2-baseline build crashes pre-Haswell CPUs at startup, Rationale: Handy shared libs ship in /usr/lib/Handy, never ldconfig-scanned /usr/lib (issue #1639), Audit Linux package runtime contents (deb/rpm/AppImage smoke test via xvfb --list-devices), Build with Tauri step (tauri-apps/tauri-action, release upload, AppImage exclusions), Audit Windows package runtime contents (NSIS/MSI install, staged DLL check, --list-devices) (+17 more)

### Community 44 - "Audio Capture"
Cohesion: 0.15
Nodes (18): AudioFrameCallback, Consumer, LevelCallback, CaptureProcessor, ChunkDisposition, Cmd, drain_available_samples(), handle_frame() (+10 more)

### Community 45 - "Community 45"
Cohesion: 0.14
Nodes (25): Backend, available_transcribe_accelerators(), AvailableAccelerators, cached_gpu_devices(), describe_compute_devices(), effective_transcribe_accelerator(), get_available_accelerators(), GPU_DEVICES (+17 more)

### Community 46 - "Community 46"
Cohesion: 0.11
Nodes (23): Dispatch, Foundation, FoundationModels, Int, Sendable, CleanedTranscript, duplicateCString(), freeAppleLLMResponse() (+15 more)

### Community 47 - "Community 47"
Cohesion: 0.13
Nodes (17): AppSettings, AutoSubmitKey, ClipboardHandling, default_post_process_prompts(), KeyboardImplementation, LLMPrompt, ModelUnloadTimeout, OrtAcceleratorSetting (+9 more)

### Community 48 - "Community 48"
Cohesion: 0.20
Nodes (23): BaseDirectory, bufreader, outputstreambuilder, soundtheme, get_sound_base_dir(), get_sound_path(), play_audio_file(), play_feedback_sound() (+15 more)

### Community 49 - "Community 49"
Cohesion: 0.15
Nodes (21): cancel_current_operation, LogLevel, openerext, cancel_operation(), get_app_dir_path(), get_app_settings(), get_default_settings(), get_log_dir_path() (+13 more)

### Community 50 - "Community 50"
Cohesion: 0.12
Nodes (18): Deref, DerefMut, default_model(), default_model_for_provider(), default_post_process_api_keys(), default_post_process_models(), default_post_process_providers(), default_selected_language() (+10 more)

### Community 51 - "Community 51"
Cohesion: 0.09
Nodes (22): compilerOptions, allowImportingTsExtensions, baseUrl, isolatedModules, jsx, lib, module, moduleResolution (+14 more)

### Community 52 - "Community 52"
Cohesion: 0.14
Nodes (15): error, ferrous_opencc, oncelock, OpenCC, chinese_script_for_locale(), ChineseVariety, convert_chinese_script(), converter() (+7 more)

### Community 53 - "Community 53"
Cohesion: 0.16
Nodes (12): BufferedFrame, frame(), Box, Option, Self, Vec, ScriptedVad, smoothed() (+4 more)

### Community 54 - "Community 54"
Cohesion: 0.30
Nodes (20): ModelInfo, cancel_download(), delete_model(), download_model(), get_available_models(), get_current_model(), get_model_info(), get_transcription_model_status() (+12 more)

### Community 55 - "Community 55"
Cohesion: 0.10
Nodes (18): GGUF_MAGIC, MAX_ARRAY_LEN, MAX_KV_COUNT, MAX_STORED_ARRAY_LEN, MAX_STRING_LEN, T_ARRAY, T_BOOL, T_FLOAT32 (+10 more)

### Community 56 - "Community 56"
Cohesion: 0.21
Nodes (15): WhatsNewPreview(), WhatsNewPreviewProps, MarkdownContent(), compareVersions(), findLatestReleaseNote(), FindReleaseNoteOptions, findReleaseNoteToShow(), parseVersion() (+7 more)

### Community 57 - "Community 57"
Cohesion: 0.26
Nodes (11): ByteCursor, ByteCursor<'a>, GgufError, parse_header(), read_value(), Display, Error, Formatter (+3 more)

### Community 58 - "Community 58"
Cohesion: 0.20
Nodes (16): Lang, detect_output_language(), detects_portuguese_sentence_containing_um(), iso639_1_for_whatlang(), langs(), MIN_CONFIDENCE, missing_metadata_detects_unconstrained(), parakeet_v3_language_list_still_detects() (+8 more)

### Community 59 - "Community 59"
Cohesion: 0.20
Nodes (13): build_gguf(), GgufMetadata, GgufValue, parses_basic_kv(), push_str(), reports_truncation_with_hint(), HashMap, Option (+5 more)

### Community 60 - "Community 60"
Cohesion: 0.13
Nodes (13): Complex32, Fft, rustfft, AudioVisualiser, CURVE_POWER, DB_MAX, DB_MIN, GAIN (+5 more)

### Community 61 - "Community 61"
Cohesion: 0.22
Nodes (13): index.html - Vite entry page mounting /src/main.tsx, react-dom, Theme, ThemeSelector, ThemeSelectorProps, installCompatShims(), objectConstructor, applyTheme() (+5 more)

### Community 63 - "Community 63"
Cohesion: 0.21
Nodes (15): anyclass, managerext, objc2_service_management, apply_autostart(), launch_agent_path_matches_auto_launch_crate(), login_item_api_available(), missing_launch_agent_is_a_no_op(), plugin_launch_agent_path() (+7 more)

### Community 64 - "Community 64"
Cohesion: 0.12
Nodes (17): scripts, build, check:model-languages, check:translations, dev, format, format:backend, format:check (+9 more)

### Community 65 - "Community 65"
Cohesion: 0.15
Nodes (14): action_map, arc, audiorecordingmanager, get_settings, is_transcribe_binding, signals, sigusr1, sigusr2 (+6 more)

### Community 66 - "Community 66"
Cohesion: 0.14
Nodes (16): AGENTS.md contributor guide, AudioManager (managers/audio.rs) - recording and device management, audio_toolkit - low-level audio processing (device enum, recording, resampling, VAD), CLI flags (--toggle-transcription, --toggle-post-process, --cancel, --start-hidden, --no-tray, --debug), Command-Event Architecture (commands frontend-to-backend, events backend-to-frontend), cpal - cross-platform audio I/O, HistoryManager (managers/history.rs) - transcription history storage, Manager Pattern (managers initialized at startup via Tauri state) (+8 more)

### Community 67 - "Community 67"
Cohesion: 0.14
Nodes (16): ModelManager (managers/model.rs) - model download and management, README.md project overview, Bluetooth headset mic reduces playback quality on macOS during recording (bidirectional audio switch), MIT-licensed code but Handy name/logo/brand assets not open-source, CLI parameters - remote control and startup flags, Custom Whisper GGML .bin model auto-discovery in models directory, Debug mode (Cmd+Shift+D macOS, Ctrl+Shift+D Windows/Linux), enigo - fallback text input library with limited Wayland compatibility (+8 more)

### Community 68 - "Community 68"
Cohesion: 0.13
Nodes (10): audio, detect_output_language, get_cpal_host, P, Self, SILERO_FRAME_MS, SILERO_FRAME_SAMPLES, SileroVad (+2 more)

### Community 69 - "Community 69"
Cohesion: 0.12
Nodes (15): Azure Trusted Signing signCommand in tauri.conf.json (release CI only), Release signature verification via minisign; pubkey in tauri.conf.json plugins.updater.pubkey, build, beforeBuildCommand, beforeDevCommand, devUrl, frontendDist, identifier (+7 more)

### Community 70 - "Community 70"
Cohesion: 0.27
Nodes (11): DownloadProgress, Fn, HttpDownloadEvent, HttpDownloadOutcome, ModelManager, CancellationToken, Option, Path (+3 more)

### Community 71 - "Community 71"
Cohesion: 0.12
Nodes (11): earshotvad, silerovad, smoothedvad, WHISPER_SAMPLE_RATE, frames_for_duration_ms(), Option, VAD_OFFLINE_HANGOVER_MS, VAD_ONSET_MS (+3 more)

### Community 72 - "Community 72"
Cohesion: 0.21
Nodes (15): macos_as_platform, CHORD_HOLD_MS, FAILED_INJECTION_TIMEOUT, failed_injection_uses_short_timeout(), finishes_after_quiet_period_once_read(), finishes_on_timeout_without_receipt(), keeps_waiting_without_receipt_within_timeout(), ownership_loss_finishes_immediately() (+7 more)

### Community 73 - "Community 73"
Cohesion: 0.12
Nodes (16): App icon 128x128, App icon 128x128 @2x (256px), App icon 32x32, App icon 64x64, App icon 512x512 (master icon), App logo 1024x1024, App icon Square 107x107 (Windows Store logo), App icon Square 142x142 (Windows Store logo) (+8 more)

### Community 74 - "Community 74"
Cohesion: 0.14
Nodes (7): FixedFrameVad, resampler_frame_size_follows_the_vad_backend(), Send, Sync, VadFrame, VadFrame<'a>, VoiceActivityDetector

### Community 75 - "Community 75"
Cohesion: 0.13
Nodes (15): Recording overlay window (overlay.rs + src/overlay frontend, platform-specific), transcribe-rs - ONNX speech recognition (Parakeet, Moonshine, SenseVoice), gtk-layer-shell runtime dependency + HANDY_NO_GTK_LAYER_SHELL opt-out, Recording overlay disabled by default on Linux - can steal focus and break paste target, Parakeet V3 - CPU-optimized model with automatic language detection, Release notes 0.9.0, Planned Legacy model deprecation with re-download upgrade path to transcribe.cpp formats, New model families - IBM Granite, Mistral Voxtral, Google MedASR, Alibaba Qwen3 ASR (+7 more)

### Community 76 - "Community 76"
Cohesion: 0.14
Nodes (15): Silero VAD (vad-rs) - voice activity detection filtering, transcribe-cpp - local Whisper-family inference (GGML/GGUF) with GPU acceleration, BUILD.md build instructions, AppImage linuxdeploy strip failure on rolling-release distros; workaround --bundles deb, deb bundle manual install method (binary + /usr/lib/Handy runtime libs on rpath), Linux build dependencies (ALSA, GTK3, WebKitGTK 4.1, appindicator, gtk-layer-shell, Vulkan tooling), Stale macOS Accessibility grant after ad-hoc rebuild; fix via tccutil reset, ONNX Runtime via Homebrew on Intel Macs (ORT_LIB_LOCATION, ORT_PREFER_DYNAMIC_LINK) (+7 more)

### Community 77 - "Community 77"
Cohesion: 0.18
Nodes (9): Detector, clamps_resampler_overshoot_without_changing_output(), EARSHOT_FRAME_SAMPLES, EarshotVad, rejects_non_finite_audio(), rejects_wrong_frame_size(), Box, Self (+1 more)

### Community 78 - "Community 78"
Cohesion: 0.13
Nodes (15): devDependencies, eslint, eslint-plugin-i18next, @playwright/test, prettier, @tauri-apps/cli, @types/node, @types/react (+7 more)

### Community 79 - "Community 79"
Cohesion: 0.18
Nodes (11): react-select, ModelSelect, ModelSelectProps, ModelOption, BaseProps, CreatableProps, NonCreatableProps, Select (+3 more)

### Community 80 - "Community 80"
Cohesion: 0.20
Nodes (12): ref_url, colorize(), colors, __dirname, getAllKeyPaths(), hasKeyPath(), LANGUAGES, loadTranslationFile() (+4 more)

### Community 81 - "Community 81"
Cohesion: 0.16
Nodes (14): AppHandle, AutoSubmitKey, ClipboardHandling, Enigo, PasteMethod, String, send_chord(), try_reliable_paste() (+6 more)

### Community 82 - "Community 82"
Cohesion: 0.19
Nodes (13): silero_vad_v4.onnx - required VAD model resource, CONTRIBUTING.md contributor guide, AI Assistance Disclosure requirement in PR descriptions, Code style guidelines (cargo fmt/clippy, strict TS no any, functional components, Tailwind), Conventional commit prefixes (feat/fix/docs/refactor/test/chore), Handy Discord community, GitHub Discussions for feature requests and community feedback, Feature freeze - new features need community support; bug fixes prioritized (+5 more)

### Community 83 - "Community 83"
Cohesion: 0.26
Nodes (12): c_char, c_int, ffi, raw, AppleLLMResponse, check_apple_intelligence_availability(), free_apple_llm_response(), is_apple_intelligence_available() (+4 more)

### Community 84 - "Community 84"
Cohesion: 0.19
Nodes (11): cpal, rtrb, TranscribeAction, AUDIO_RING_SECONDS, CONSUMER_POLL_INTERVAL, is_microphone_access_denied(), is_no_input_device_error(), MAX_DRAIN_CHUNK (+3 more)

### Community 85 - "Community 85"
Cohesion: 0.21
Nodes (12): ModelUnloadTimeout, serialize, get_model_load_status(), ModelLoadStatus, AppHandle, Option, State, String (+4 more)

### Community 86 - "Community 86"
Cohesion: 0.27
Nodes (12): build_apple_intelligence_bridge(), camel_to_snake(), escape_string(), generate_tray_translations(), is_command_line_tools_only(), main(), Option, String (+4 more)

### Community 87 - "Community 87"
Cohesion: 0.22
Nodes (5): Duration, Instant, STREAM_FINALIZE_REPLY_TIMEOUT, STREAM_PERF_LOG_INTERVAL, StreamPerf

### Community 88 - "Community 88"
Cohesion: 0.27
Nodes (12): Handy app icon (logo), Handy app icon with warning badge, Recording state icon (overlay/UI), Transcribing state icon (overlay/UI), Tray icon: idle state (light variant), Tray icon: idle state (dark variant), Tray icon: idle with warning (light variant), Tray icon: idle with warning (dark variant) (+4 more)

### Community 89 - "Community 89"
Cohesion: 0.24
Nodes (6): CancelAction, AppHandle, Send, Sync, ShortcutAction, TestAction

### Community 90 - "Community 90"
Cohesion: 0.24
Nodes (9): BuildStreamError, Producer, acknowledge_pause_after_write(), CaptureTransportState, AtomicBool, AtomicU64, T, Stream (+1 more)

### Community 91 - "Community 91"
Cohesion: 0.31
Nodes (8): canonicalizeNodeModules(), collectLinks(), isDirectory(), LinkEntry, BinEntry, collectBinLinks(), isDirectory(), normalizeBunBinaries()

### Community 92 - "Community 92"
Cohesion: 0.29
Nodes (10): BinSpec, binTarget(), defaultBinName(), HealedEntry, healPeerDepBins(), isDirectory(), Manifest, parseBinField() (+2 more)

### Community 93 - "Community 93"
Cohesion: 0.47
Nodes (7): default_quant_file(), DiskStatus, ModelDescriptor, ModelSource, QuantFile, Option, test_catalog_quant_rendering()

### Community 94 - "Community 94"
Cohesion: 0.29
Nodes (7): evaluate(), Instant, Option, Self, Vec, TxState, WaitDecision

### Community 95 - "Community 95"
Cohesion: 0.20
Nodes (10): Settings system (settings.rs, settingsStore.ts Zustand, tauri-plugin-store persistence), shortcut.rs - global keyboard shortcut handling, Ubuntu 26.04 GNOME Wayland troubleshooting guide (Handy 0.9.8), handy_keys keyboard implementation for shortcuts, settings_store.json keys typing_tool=ydotool and keyboard_implementation=handy_keys, /dev/uinput + input group requirement (udev 80-uinput.rules), ydotool typing fix for Wayland (systemd user service), Press/Speak/Release/Paste workflow (hold, toggle, hold-only, toggle-only modes) (+2 more)

### Community 96 - "Community 96"
Cohesion: 0.20
Nodes (3): CancelIconProps, MicrophoneIconProps, TranscriptionIconProps

### Community 97 - "Community 97"
Cohesion: 0.20
Nodes (10): app, macOSPrivateApi, security, windows, enable, scope, allow, requireLiteralLeadingDot (+2 more)

### Community 98 - "Community 98"
Cohesion: 0.31
Nodes (9): ApplicationWindow, OverlayPosition, PhysicalPosition, PhysicalSize, configure_layer_shell_position(), configure_layer_shell_surface(), is_mouse_within_monitor(), windows_overlay_bounds() (+1 more)

### Community 99 - "Community 99"
Cohesion: 0.22
Nodes (9): CanaryModel, CohereModel, GigaAMModel, MoonshineModel, ParakeetModel, SenseVoiceModel, Session, LoadedEngine (+1 more)

### Community 100 - "Community 100"
Cohesion: 0.31
Nodes (7): D, deserialize_transcribe_gpu_device(), LogLevel, Error, From, secret_map_debug_redacts_values(), tauri_plugin_log::LogLevel

### Community 101 - "Community 101"
Cohesion: 0.28
Nodes (8): Debug, hound, path, read_wav_samples(), P, Vec, save_wav_file(), verify_wav_file()

### Community 102 - "Community 102"
Cohesion: 0.33
Nodes (4): Progress, HfDownloadProgress, HfProgressState, Instant

### Community 103 - "Community 103"
Cohesion: 0.22
Nodes (8): ref_fs, currentHash, hashFile, lockFile, nixDir, nixFile, result, root

### Community 104 - "Community 104"
Cohesion: 0.25
Nodes (8): Bug Report issue template, Issue template chooser config (blank issues disabled), GitHub Discussions contact links (feature requests redirected), AI PR policy: no automated PRs, human-written description and AI disclosure required, GitHub Discussions for community feedback on PRs, Feature freeze policy: bug fixes prioritized, unrequested features rejected, Pull Request template, CONTRIBUTING.md contributor guide

### Community 105 - "Community 105"
Cohesion: 0.25
Nodes (8): bun.nix sync check (bun2nix regeneration must match committed .nix/bun.nix), Cachix binary cache push (handy-computer cache), Flake evaluation + conditional full nix build (~25 min, only on nix packaging changes), nix-build job (bun.nix sync check, flake eval, conditional full nix build), Trigger: pull_request (nix/source paths), Trigger: push to main (nix/source paths), Trigger: workflow_dispatch (manual, always full build), nix build check workflow

### Community 106 - "Community 106"
Cohesion: 0.29
Nodes (6): react-markdown, allowedElements, components, isSafeUrl(), MarkdownContentProps, openSafeUrl()

### Community 107 - "Community 107"
Cohesion: 0.25
Nodes (7): compilerOptions, allowSyntheticDefaultImports, composite, module, moduleResolution, skipLibCheck, include

### Community 108 - "Community 108"
Cohesion: 0.33
Nodes (6): Quality gate checks: translations, model language coverage, keyboard tests, ESLint, Prettier, code-quality job, Trigger: pull_request (same path filters), Trigger: push to main (path-filtered src/, package.json, bun.lock, configs), Trigger: workflow_dispatch (manual), code quality workflow

### Community 109 - "Community 109"
Cohesion: 0.33
Nodes (6): cargo test run in src-tauri, rust-tests job (ubuntu-24.04, Vulkan deps for transcribe-cpp-sys), Trigger: pull_request (src-tauri/** paths), Trigger: push to main (src-tauri/** paths), Trigger: workflow_dispatch (manual), test workflow (Rust tests)

### Community 110 - "Community 110"
Cohesion: 0.33
Nodes (6): i18next internationalization system (src/i18n/locales, en translation.json source), CONTRIBUTING_TRANSLATIONS.md translation guide, Supported languages (en source; ca, zh, fr, de, ja, es, vi complete; ko, pt requested), src/i18n/languages.ts LANGUAGE_METADATA language registration, src/i18n/locales/<lang>/translation.json per-language structure (ISO 639-1 codes), Translation rules - translate values not keys, preserve {{variables}} and brand names

### Community 111 - "Community 111"
Cohesion: 0.33
Nodes (5): description, identifier, permissions, $schema, windows

### Community 112 - "Community 112"
Cohesion: 0.33
Nodes (4): DownloadCleanup<'a>, RescanGuard, AtomicBool, Drop

### Community 113 - "Community 113"
Cohesion: 0.40
Nodes (5): Android launcher icon hdpi, Android launcher icon mdpi, Android launcher icon xhdpi, Android launcher icon xxhdpi, Android launcher icon xxxhdpi

### Community 114 - "Community 114"
Cohesion: 0.40
Nodes (5): Android launcher icon foreground hdpi, Android launcher icon foreground mdpi, Android launcher icon foreground xhdpi, Android launcher icon foreground xxhdpi, Android launcher icon foreground xxxhdpi

### Community 115 - "Community 115"
Cohesion: 0.40
Nodes (5): Android launcher icon round hdpi, Android launcher icon round mdpi, Android launcher icon round xhdpi, Android launcher icon round xxhdpi, Android launcher icon round xxxhdpi

### Community 116 - "Community 116"
Cohesion: 0.50
Nodes (4): playwright job (chromium install, test:playwright, report upload on failure), Trigger: pull_request (src/, tests/, playwright.config paths), Trigger: workflow_dispatch (manual), Playwright workflow

### Community 117 - "Community 117"
Cohesion: 0.50
Nodes (4): iOS AppIcon 20pt @1x, iOS AppIcon 20pt @2x, iOS AppIcon 20pt @2x (alt), iOS AppIcon 20pt @3x

### Community 118 - "Community 118"
Cohesion: 0.50
Nodes (4): iOS AppIcon 29pt @1x, iOS AppIcon 29pt @2x, iOS AppIcon 29pt @2x (alt), iOS AppIcon 29pt @3x

### Community 119 - "Community 119"
Cohesion: 0.50
Nodes (4): iOS AppIcon 40pt @1x, iOS AppIcon 40pt @2x, iOS AppIcon 40pt @2x (alt), iOS AppIcon 40pt @3x

### Community 122 - "Community 122"
Cohesion: 0.67
Nodes (3): hide_recording_overlay(), cancel_current_operation(), AppHandle

## Knowledge Gaps
- **508 isolated node(s):** `LinkEntry`, `Manifest`, `BinSpec`, `HealedEntry`, `BinEntry` (+503 more)
  These have ≤1 connection - possible missing edges or undocumented components. (Counts symbols only; 991 node(s) total have ≤1 connection when file, concept and rationale nodes are included.)
- **20 thin communities (<3 nodes) omitted from report** — run `graphify query` to explore isolated nodes.

## Suggested Questions
_Questions this graph is uniquely positioned to answer:_

- **Why does `react` connect `Settings UI (Misc)` to `Community 96`, `Settings UI Components`, `Model Selector UI`, `Tauri Bindings`, `App Shell & Onboarding`, `Community 106`, `Community 79`, `Debug & About UI`, `Footer & Misc UI`, `Community 56`, `Debug UI Components`, `Frontend Tooling Config`, `Community 61`?**
  _High betweenness centrality (0.029) - this node is a cross-community bridge._
- **Why does `get_settings()` connect `Audio Commands` to `Shortcuts & Settings`, `Streaming Transcription`, `Clipboard & Paste`, `Model Manager`, `Settings Backend`, `Hotkey Handling`, `Audio Recording Manager`, `Community 47`, `Recording Overlay`, `Community 49`, `Community 50`, `Community 84`, `Community 85`, `Transcription Actions`, `Community 54`, `App Initialization`, `Transcription Engine Loading`?**
  _High betweenness centrality (0.027) - this node is a cross-community bridge._
- **Why does `TxState` connect `Community 94` to `macOS Paste`, `Community 72`, `Windows Paste`?**
  _High betweenness centrality (0.021) - this node is a cross-community bridge._
- **What connects `LinkEntry`, `Manifest`, `BinSpec` to the rest of the system?**
  _508 weakly-connected nodes found - possible documentation gaps or missing edges._
- **Should `Shortcuts & Settings` be split into smaller, more focused modules?**
  _Cohesion score 0.05623762376237624 - nodes in this community are weakly interconnected._
- **Should `Transcription Coordinator` be split into smaller, more focused modules?**
  _Cohesion score 0.05689548546691404 - nodes in this community are weakly interconnected._
- **Should `Settings UI Components` be split into smaller, more focused modules?**
  _Cohesion score 0.07946735395189003 - nodes in this community are weakly interconnected._