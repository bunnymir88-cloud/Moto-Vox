# MotoVox — status & verification record

**Legend:** ✅ implemented in source · 🟡 partial · ⛔ not implemented · "unverified" = never compiled or run.

## What was actually verified (in the generation environment: Java 21, no Android SDK/Gradle, no network)
| Check | Result |
|---|---|
| All XML/SVG resources parse as well-formed | ✅ passed (12 files) |
| Delimiter-balance lint over 36 Kotlin files | ✅ passed (one false positive from a nested-quote template, inspected by hand) |
| DSP algorithms prototyped in Python (`tools/dsp_prototype_check.py`) | ✅ pitch shifter within 0.5% of target at ±12 st; limiter holds ceiling; 90 Hz high-pass −3 dB at 90 Hz; WAV header = 44 bytes. **This validates the math, not the Kotlin code.** |
| Logo rendered at 1024/512/256/96/64/48/36 px and under circle + rounded-square masks | ✅ inspected visually (`branding/preview_sheet.png`) |
| Kotlin compilation | ❌ **not possible here — never compiled** |
| Unit tests (`app/src/test`, 4 files, ~40 tests) | ❌ **written, never run** |
| Instrumented tests (`app/src/androidTest`) | ❌ written, never run |
| Debug / release APK | ❌ **not built — no APK exists** |
| Any hardware (built-in, wired, USB, Bluetooth) | ❌ untested |

## Feature checklist
**Recording** — ✅ AudioRecord PCM capture → WAV (16-bit) · ✅ 48/44.1 kHz, mono/stereo · ✅ pause/resume (mic released while paused) ·
✅ capture thread → queue → writer thread · ✅ header flushed every ~5 s + startup repair of interrupted files ·
✅ foreground service (type microphone) with Pause/Mark/Stop · ✅ auto-splitting (15/30/60 min) · ✅ low-storage auto-stop & save ·
✅ clip detection · ✅ software gain (🟡 hardware gain: not exposed by Android on all phones, so not offered) ·
🟡 interruptions: a "no signal for 3 s" warning covers calls/another app taking the mic; no audio-focus handling ·
⛔ resume after device reboot (Android doesn't allow background mic start) · ⛔ live monitoring (latency/feedback risk) ·
⛔ AAC while recording (AAC is an export step) · 🟡 markers in a recording killed by the OS are lost (audio is recovered).

**Inputs** — ✅ device enumeration with live add/remove updates · ✅ preferred-device selection · ✅ actual routed device shown, with an
honest fallback note · ✅ Bluetooth via setCommunicationDevice (API 31+) / SCO (older), only when the user picks a BT input; explained as
lower quality · ✅ mic test (meter, 8 s test, playback, route, rate/channels as delivered, clipping verdict, troubleshooting) · all unverified on hardware.

**Processing** — ✅ high-pass, wind-rumble filter (4th-order low cut; cannot remove speech-band wind), light noise gate, 3 EQ presets,
compressor, limiter, gain · 🟡 no spectral noise reduction (not implemented; no dead control is shown) · non-destructive, saved as new files · 
🟡 A/B compare = play original vs first 12 s processed.

**Voice FX** — ✅ Original, Deep, Low, High, Chipmunk, Robot, Radio, Echo, Reverb, Monster, Custom (pitch + echo) with intensity ·
granular pitch shifter (not speed change; artefacts at extremes) · output is mono WAV.

**Music & Mix** — ✅ document-picker import, MP3/AAC/WAV etc. via platform decoder · ✅ voice/music gain, ducking, trim, offset, loop, fades, limiter ·
✅ 5 presets that set real parameters · ✅ 20 s preview · ✅ save mix as new Library item · 🟡 music files are not kept as Library
"references" (they are picked per mix; decoded cache lives in app cache) · 🟡 mix length = voice length.

**Markers & sync** — ✅ big Mark button, notification Mark action, per-file marker lists, CSV/JSON export, start timestamp & duration in metadata ·
✅ optional 3-beep sync tone (+ automatic marker) · ⛔ spoken countdown/label · ⛔ automatic video sync (deliberately not claimed).

**Library & export** — ✅ search, sort (date/name/duration), favorite, rename, notes, copy, delete-with-confirm, storage usage, missing-file detection ·
✅ export WAV, M4A (AAC-LC 96/160 kbps), markers CSV/JSON, metadata JSON via system file picker with progress/cancel/result summary ·
⛔ share-sheet (ACTION_SEND) · 🟡 index is a JSON file, not Room (deliberate: fewer moving parts).

**Settings** — ✅ sample rate, channels, software gain, split length, sync tone, name prefix, theme (dark/light/system), storage info,
permission diagnostics, battery-optimization shortcut, troubleshooting, about · "Recording format" is shown as fixed WAV (honest, not a fake toggle).

**Branding** — ✅ adaptive icon (foreground + background + monochrome) as vector XML · ✅ splash (androidx splashscreen) · ✅ in-app logo/wordmark ·
✅ SVG + 1024/512 PNG + preview sheet · ⛔ legacy PNG mipmaps (not needed at minSdk 26) · design is a first pass: the red wheel arc can read as a smile.

**Build deliverables** — ✅ Gradle Kotlin DSL project, docs · ⛔ **gradle-wrapper.jar / gradlew scripts** (cannot be generated offline) ·
⛔ **APK of any kind**.

## Likely first-build issues (honest expectations)
Compose/Material API drift, a missed import, nullability at an Android API boundary, or lint "NewApi" on an inlined constant. Nothing
structural is expected, but treat the first build as a debugging session.
