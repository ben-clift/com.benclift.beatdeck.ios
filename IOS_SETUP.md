# BeatDeck for iPhone: status and setup

## What works now
- The whole app (Flutter screens, Soulseek, Auto DJ, mixing logic) builds for iPhone.
- The C++ DJ engine is built into the app (`native/ios`): the Android-only audio
  layer (Oboe) is replaced by a small RemoteIO shim (`native/ios/shim/oboe/Oboe.h`),
  and M4A/AAC/ALAC decoding uses Apple's AudioToolbox (`native/engine/apple_aac.hpp`).
- The Mac build (`.github/workflows/ios.yml`) produces an unsigned `Runner.app` and
  checks that the engine functions are inside the app.

## Not done / not tested on a real iPhone yet
- Nothing has run on a device. Needs a signed build (below).
- Shazam button (Android reads notifications; on iPhone use ShazamKit later).
- "Open another app" (returns false on iPhone), USB output (Android only).
- Possible runtime issues: audio route changes/interruptions (calls, Siri) are not handled yet.

## Getting it onto a phone (TestFlight)
You need an Apple Developer account (about £79/year). Then add these GitHub
secrets to this repo (Settings > Secrets and variables > Actions):

| Secret | What it is |
|---|---|
| `IOS_CERT_P12_BASE64` | Distribution certificate exported as .p12, base64 encoded |
| `IOS_CERT_PASSWORD` | Password you set when exporting the .p12 |
| `IOS_PROFILE_BASE64` | App Store provisioning profile for `com.benclift.beatdeck.ios`, base64 encoded |
| `ASC_KEY_ID` | App Store Connect API key id |
| `ASC_ISSUER_ID` | App Store Connect issuer id |
| `ASC_KEY_P8_BASE64` | The API key (.p8 file), base64 encoded |

Also create the app in App Store Connect with bundle id `com.benclift.beatdeck.ios`.
When the secrets exist, each push builds a signed IPA and uploads it to TestFlight.
Friends then install through the TestFlight app.
