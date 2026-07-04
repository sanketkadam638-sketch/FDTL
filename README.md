# Cabin Crew FDTL Calculator — Android App

This folder is a ready-to-build **Capacitor** Android project. It wraps the offline
FDTL calculator (`www/index.html`) as a native Android app you can open in Android
Studio, build, and publish to the Play Store.

The app has no network calls at all (no fonts, no APIs) — it will work with the
device fully in airplane mode.

## What you need first (one-time setup)

1. **Install Node.js** (v18 or newer) — https://nodejs.org
2. **Install Android Studio** — https://developer.android.com/studio
   - On first launch, let it install the Android SDK, Android SDK Platform-Tools,
     and an Android Virtual Device (or just plan to test on your own phone via USB).
3. **Create a Google Play Developer account** (one-time $25 fee) —
   https://play.google.com/console/signup

## Building the app

Open a terminal in this folder (`fdtl-app/`) and run:

```bash
npm install
npx cap sync android
```

Then either:

**Option A — open in Android Studio (recommended for your first build)**
```bash
npx cap open android
```
This launches Android Studio with the project loaded. From there:
- Click the green ▶ Run button to test on an emulator or a phone connected by USB
  (enable "USB debugging" in your phone's Developer Options first).
- When ready to publish: `Build → Generate Signed Bundle / APK`, choose
  **Android App Bundle (.aab)**, create a signing key (Android Studio walks you
  through this — save that keystore file somewhere safe, you'll need the *same*
  one for every future update), and build the release `.aab`.

**Option B — command line**
```bash
cd android
./gradlew bundleRelease
```
(You'll still need a signing key set up in `android/app/build.gradle` for a
release build — Android Studio's guided flow above is easier the first time.)

## Publishing to the Play Store

1. Log in to the [Play Console](https://play.google.com/console).
2. Create a new app, fill in the store listing (title, description, screenshots,
   privacy policy URL, content rating questionnaire, data safety form).
3. Upload the signed `.aab` from above to a testing or production track.
4. Submit for review. Google's review usually takes a few hours to a few days.

### Things you'll be asked for during listing
- **App icon** (512×512 PNG) and **feature graphic** (1024×500 PNG) — not yet
  included in this project; you'll want to design these or ask me to generate
  placeholder versions.
- **Screenshots** of the app running (take these from the emulator or your phone).
- **Privacy policy URL** — required even for offline apps with no data
  collection; a one-page "this app collects no data" policy hosted anywhere
  (e.g. a GitHub Pages page or Google Site) satisfies this.
- **Content rating questionnaire** — this is a reference/utility tool with no
  objectionable content, so it should rate as "Everyone" straightforwardly.

## Updating the app content later

If you edit `www/index.html` (the calculator itself), re-run:
```bash
npx cap sync android
```
before rebuilding, so Android Studio picks up the changes.

## Notes

- The manifest currently includes the `INTERNET` permission — this is added by
  Capacitor by default for the WebView component itself, not because the app
  calls out to any server. The calculator logic runs 100% on-device.
- App ID is set to `com.fdtlcalc.cabincrew` and app name to "Cabin Crew FDTL" —
  both editable in `capacitor.config.ts` and `android/app/src/main/res/values/strings.xml`
  before your first release (the app ID cannot be changed after you publish).
