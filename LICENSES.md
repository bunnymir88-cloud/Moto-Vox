# Licenses & third-party notices

**MotoVox source code**: no license has been chosen yet. Add one before distributing.

**Dependencies** (all Apache-2.0, fetched from Google Maven / Maven Central at build time):
AndroidX Core, Core-SplashScreen, Activity, Lifecycle, Navigation, DataStore; Jetpack Compose (UI, Material 3, Material Icons Extended);
Kotlin standard library and kotlinx-coroutines. Test only: JUnit 4 (EPL-1.0), org.json (JSON License), AndroidX Test (Apache-2.0).

**DSP**: all effects (biquad filters from the public "Audio EQ Cookbook" formulas, compressor, limiter, gate, echo, Schroeder reverb,
ring modulator, granular pitch shifter) are original implementations in this repo. No third-party audio libraries are bundled.

**Audio decoding/encoding** uses the Android platform (MediaCodec/MediaExtractor/MediaMuxer). Codec availability and any codec
patent/licensing considerations depend on the device and its manufacturer.

**Branding**: the MotoVox logo is original to this project. The name "MotoVox" has **not** been checked for trademark clearance.
**Music**: none bundled. Only import music you have the rights to use.
