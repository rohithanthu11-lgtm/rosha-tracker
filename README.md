# ROSHA TRACKER — Capacitor source project

This is the **source project**, not a precompiled APK, AAB, or iOS IPA.
It packages the existing ROSHA TRACKER HTML/CSS/JavaScript app for building with Capacitor.

## Requirements
- Node.js LTS and npm
- Android build: Android Studio + Android SDK
- iOS build: macOS + Xcode (Apple tooling is required; iOS builds cannot be compiled on Windows/Android)
- For App Store distribution: Apple Developer account/signing as required by Apple

## 1. Install dependencies
Open a terminal in this project folder:
```bash
npm install
```

## 2. Generate native platform projects
```bash
npx cap add android
npx cap add ios
```
If either command says the platform already exists, do not run it again.

## 3. Sync web app into native projects
Run this after editing files in `www/`:
```bash
npx cap sync
```

## 4. Build Android APK
```bash
npx cap open android
```
In Android Studio, wait for Gradle sync, then use **Build > Build Bundle(s) / APK(s) > Build APK(s)**.
For a release APK, configure signing in Android Studio. For Play Store, build a signed Android App Bundle (AAB).
Typical debug APK output after a successful build:
`android/app/build/outputs/apk/debug/app-debug.apk`
The output path is created only after the native build succeeds.

## 5. Build iOS app
On a Mac with Xcode:
```bash
npx cap open ios
```
Select an iOS Simulator or connected iPhone, configure Signing & Capabilities, then Run. For distribution, archive and export/sign via Xcode.
A real installable IPA is not included in this source ZIP.

## 6. Run the web app
You can open `www/index.html` for a quick local preview. To test service-worker/PWA install features, host the `www` folder on HTTPS. Or run:
```bash
npm run web
```
Then use the local URL printed by the server.

## Local-only data
Habits and completion history are stored in browser/device local storage. There is no account, cloud sync, or cross-device synchronization. Browser storage and native app storage are separate containers, so web data will not automatically transfer into the installed Android/iOS app. Use the app's Export my data / Import JSON feature to move backups manually.

## Current app features
Daily habit list, add/edit/pause/archive, daily/weekday/weekend/custom schedules, calendar, daily and monthly progress, local JSON export/import, display name and reminder-time preference.

## Important
The reminder time is a saved preference only; this source does not schedule native background notifications. Notifications require native notification scheduling and permissions to be implemented separately.

## Build an installable APK in GitHub Actions (no Android Studio on your phone needed)

This source ZIP is **not itself an APK**. It now includes `.github/workflows/build-android-apk.yml` so GitHub can compile a debug APK in its hosted Android/Java build environment.

1. On GitHub, create a new **private** repository named `rosha-tracker`.
2. Extract this ZIP on a computer and upload the *contents* of the `ROSHA_TRACKER_Capacitor_Source` folder to the repository root. Make sure `.github/workflows/build-android-apk.yml` is included. (On mobile, use GitHub's website in desktop mode or a computer; hidden `.github` folders must be uploaded too.)
3. Commit the files to the `main` branch.
4. Open the repository's **Actions** tab and choose **Build ROSHA TRACKER APK**. If prompted, enable workflows, then press **Run workflow**.
5. Wait for the run to finish with a green check. Open that run, scroll to **Artifacts**, and download `rosha-tracker-debug-apk`.
6. Extract the downloaded artifact ZIP. It contains `app-debug.apk`. Transfer it to your Android phone and tap it to install. Android may ask you to allow installs from that source; only enable this for files you trust.

This produces a **debug APK for testing**, not a Play Store release. A signed release APK/AAB needs release signing configuration. The workflow compiles the existing local-only app; it does not add cloud sync or native scheduled notifications.
