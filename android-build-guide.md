# Build Android APK & Play Store AAB

## Option A: Cloud Build (No PC / Android Studio Needed)
1. Upload this extracted folder to a GitHub repository.
2. Open the **Actions** tab and run **Build Android APK & Play Store AAB**.
3. Download ready-to-install `app-debug.apk` and Google Play `app-release.aab`!

## Option B: Local Android Studio
```bash
npm install
npx cap add android
npx cap sync android
npx cap open android
```
In Android Studio: **Build > Build Bundle(s) / APK(s) > Build APK(s) or Build Bundle(s)**.