# MotoVox — Ride. Record. Speak.

MotoVox turns your Android phone into an offline audio recorder for motorcycle vlogging. Record your voice from a wired, USB or Bluetooth mic while your action camera films. Mark moments, reduce wind rumble, add voice effects, mix in music, and export WAV or M4A to sync in your editor. No account or internet needed.

Offline Android audio recorder, processor and mixer for motorcycle vloggers. Your action camera records video; MotoVox records
your voice separately (phone mic, wired/USB mic, or a compatible Bluetooth mic), then helps you clean it up, add music and export
audio to sync in your video editor. **It is an independent audio companion, not an action-camera replacement.**

> **Build status: NOT BUILT.** This project was generated in an environment with no Android SDK, no Gradle and no network, so it
> has **never been compiled, run or installed**. See `STATUS.md` for exactly what is and isn't verified, and
> `BUILD_INSTRUCTIONS.md` to build it. Expect to fix a small number of compile errors on first build.

![preview](branding/preview_sheet.png)

## Quick start: get the APK without installing anything
1. Create a free, private GitHub repository and upload everything in this folder (including the hidden `.github` folder) to `main`.
2. Open the **Actions** tab, run **Build MotoVox APK**, and download the **MotoVox-debug-apk** artifact.
3. Unzip it, copy `app-debug.apk` to your phone, open it and allow installs from that source.
4. Grant microphone permission, connect your mic, and run **Record → Mic test** before your first ride.

Prefer Android Studio or the Windows command line? See `BUILD_INSTRUCTIONS.md`. If the cloud build fails, copy the red error lines from the log; the code hasn't been compiled yet, so small fixes may be needed.

## Features (see STATUS.md for the honest checklist)
- **Record**: WAV 16-bit PCM at 48/44.1 kHz, mono/stereo; pause/resume; foreground service (screen-off); live dBFS meter with
  clip detection; software gain; real input-device selection and route reporting; free-space/size estimates; auto file-splitting;
  sync tone; one-tap ride markers; crash recovery of interrupted recordings.
- **Mic test**: input list with reported rates/channels, live meter, 8-second test + playback, route confirmation, clipping verdict.
- **Voice FX** (11 effects, intensity + custom pitch) and **clean-up** (software gain, high-pass, wind-rumble filter, light noise
  gate, EQ presets, compressor, limiter). Non-destructive: results are saved as new recordings.
- **Music & Mix**: import music (document picker), voice/music levels, ducking, trim, offset, loop, fades, limiter, 5 presets,
  20-second preview, save mix as a new file.
- **Library**: search, sort, favorites, rename, copy, notes, markers, delete with confirmation, missing-file detection.
- **Export** (system file picker): WAV, compact M4A (AAC-LC), markers CSV/JSON, project metadata JSON.
- Fully offline. No internet permission, accounts, ads or analytics.

## Requirements
Android 8.0+ (API 26). Build: JDK 17+, Android SDK 34, Gradle 8.7 / AGP 8.5.2 / Kotlin 1.9.24.

## Permissions (all requested in context)
| Permission | Why |
|---|---|
| `RECORD_AUDIO` | Record your voice |
| `FOREGROUND_SERVICE`, `FOREGROUND_SERVICE_MICROPHONE` | Keep recording with screen off / app backgrounded |
| `POST_NOTIFICATIONS` (13+) | Recording notification with Pause / Mark / Stop |
| `BLUETOOTH_CONNECT` (12+), `BLUETOOTH` (≤11), `MODIFY_AUDIO_SETTINGS` | Route to a Bluetooth headset mic (only when you select one) |

No location, contacts, SMS or storage permissions. Recordings live in app-private storage; exports use the system file picker.

## Safety
Set everything up while stopped. Don't operate your phone while riding.

## Privacy & known limitations
Audio never leaves the phone and is never logged. Limits: recordings are not encrypted at rest; any app with root/ADB access can read
them; uninstalling MotoVox deletes its private recordings (export what you want to keep); notification text shows your project name.

## Sync with camera video
Phone and camera clocks and start delays differ. Use the optional sync tone or a clap at the start of both recordings and align the
spike in your editor. MotoVox does **not** claim frame-perfect automatic sync.

## Layout
`app/src/main/java/com/motovox/app/` — `audio/` (WAV I/O, devices, capture) · `recording/` (engine, service, mic test) ·
`effects/` (DSP + chains) · `mixer/` (mix, music decoder) · `export/` · `data/` · `permissions/` · `ui/` (Compose).
Branding: `branding/` (SVG + PNG + preview), `tools/gen_logo.py` regenerates the Android vector drawables from one source.
