# Hardware test plan (none of this has been performed)
Record device model, Android version, accessory, result, and any fallback message for each row.
| # | Test | Pass criteria |
|---|---|---|
| 1 | Built-in mic, 48 kHz mono, 5 min | Playable WAV; meter moves; no clipping at normal speech |
| 2 | Screen off 30 min | Notification persists; file continuous; duration matches wall clock ±2 s |
| 3 | Pause/resume x5 | Elapsed time excludes paused periods; no glitches at joins |
| 4 | Wired 3.5 mm TRRS headset mic | Listed as "Wired headset"; Mic test route confirms it |
| 5 | USB-C headset / USB mic | Listed as "USB audio"; route confirmed; try 48 and 44.1 kHz |
| 6 | Output-only adapter | Mic test reports no signal / not listed; message is clear |
| 7 | Bluetooth headset (SCO/BLE) | Route shows Bluetooth, or MotoVox reports the fallback honestly |
| 8 | Unplug mic mid-recording | Route note appears; audio so far is saved |
| 9 | Incoming phone call | Silence warning appears; file still finalizes |
| 10 | Force-stop app mid-recording | Next launch: recording appears as "recovered", plays |
| 11 | Fill storage during recording | Recording stops and saves; message shown |
| 12 | 2 h recording with 30-min splitting | 4+ files, no gap > 1 buffer; markers land in right part |
| 13 | Mix with 10-min MP3 | Completes; no clipping; original files unchanged |
| 14 | Export WAV/M4A to Drive/Downloads | File opens in a video editor |
| 15 | Android 13/14 permission flows | Rationale shown; denial handled; notification denial doesn't block recording |
