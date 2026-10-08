# Building MotoVox

> The project has not been compiled by its author. If the first build reports errors, they will be small (imports, nullability, API
> signatures). Open an issue-style list from the Gradle output and fix them; the architecture is intentionally conventional.

## A. Android Studio (recommended, any OS)
1. Install **Android Studio** (current stable) — it bundles JDK 17+ and the SDK manager.
2. *File → Open* → select the `MotoVox` folder. Accept "Trust project".
3. Let Gradle sync. Studio will offer to download **Android SDK Platform 34** and build tools — accept.
   The Gradle *wrapper JAR is not included* (it can't be generated offline). If Studio doesn't create it, open a terminal in the project
   root and run `gradle wrapper --gradle-version 8.7` once (needs a local Gradle), or use Studio's bundled Gradle.
4. *Build → Make Project*. Fix any compile errors reported.
5. Run unit tests: right-click `app/src/test` → *Run Tests*, or `./gradlew testDebugUnitTest`.
6. *Build → Build Bundle(s)/APK(s) → Build APK(s)*. Output: `app/build/outputs/apk/debug/app-debug.apk` (rename to `MotoVox-debug.apk` if you like).

## B. Windows command line
```bat
:: JDK 17+ and Android SDK installed; set ANDROID_HOME (e.g. C:\Users\you\AppData\Local\Android\Sdk)
:: create local.properties with:  sdk.dir=C\:\\Users\\you\\AppData\\Local\\Android\\Sdk
gradlew.bat testDebugUnitTest
gradlew.bat assembleDebug
:: -> app\build\outputs\apk\debug\app-debug.apk
```
(If `gradlew.bat` is missing, run `gradle wrapper --gradle-version 8.7` first, see above.)

## C. Install on your phone from Windows
1. Copy `app-debug.apk` to the phone (USB cable, cloud drive, or messaging to yourself).
2. Open it with the phone's file manager.
3. If Android asks, allow installing from that source ("Install unknown apps"). Menu names vary by manufacturer.
4. Confirm **Install**, then **Open** MotoVox.
5. Grant **microphone** permission when asked (and notifications on Android 13+).
6. Connect your microphone (USB-C/3.5 mm TRRS adapter, or pair Bluetooth first).
7. Open **Record → Mic test**, choose the input, run the 8-second test and play it back.
8. Check the reported route/sample rate and the verdict. Then make a first real recording and verify playback in the Library.

Or with USB debugging: `adb install -r app\build\outputs\apk\debug\app-debug.apk`.

## D. Signed release build (never commit secrets)
```
keytool -genkeypair -v -keystore motovox-release.jks -alias motovox -keyalg RSA -keysize 4096 -validity 10000
```
Create `keystore.properties` in the project root (git-ignored):
```
storeFile=C:\\secure\\motovox-release.jks
storePassword=...
keyAlias=motovox
keyPassword=...
```
Then `gradlew assembleRelease` → `app/build/outputs/apk/release/app-release.apk`. Without `keystore.properties` the release APK is **unsigned**
(`app-release-unsigned.apk`) and can't be installed until signed. Back up the keystore: losing it means you can't update the app.

## E. Tests
- Unit (JVM, no device): `./gradlew testDebugUnitTest` — see STATUS.md for what they cover. **They have not been run yet.**
- Instrumented (device/emulator with mic): `./gradlew connectedDebugAndroidTest`.
- Hardware routes (wired/USB/Bluetooth): manual, see `HARDWARE_TEST_PLAN.md`.

## F. Troubleshooting the build
- *SDK location not found*: create `local.properties` with `sdk.dir=...`.
- *Unsupported Java*: use JDK 17 or 21 for Gradle; the project targets Java 17 bytecode.
- *Compose compiler/Kotlin mismatch*: Kotlin 1.9.24 pairs with Compose compiler 1.5.14 (set in `app/build.gradle.kts`). If you upgrade Kotlin, change both.
- *Missing material icons*: `material-icons-extended` is declared; re-sync.

## G. No PC setup: build the APK in the cloud (GitHub Actions)
1. Create a free GitHub account and a new **private** repository.
2. Upload the contents of this project (everything inside the `MotoVox` folder, including the hidden `.github` folder) to the repo's `main` branch.
3. Open the repo's **Actions** tab → **Build MotoVox APK** → it runs automatically (or press **Run workflow**).
4. When it finishes (a few minutes), open the run and download the **MotoVox-debug-apk** artifact (a zip containing `app-debug.apk`).
5. If the run fails, download **build-reports** or copy the red error lines from the log. The code has never been compiled, so the first run may fail with small compile errors.
