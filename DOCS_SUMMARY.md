# Documentation Summary - My Coach Finder Android App

**Generated:** October 27, 2025
**Platform:** Android Only
**Status:** Production Ready

---

## Overview

My Coach Finder Android app is a native mobile application that provides access to Germany's leading coaching platform. Built with Capacitor 6.x, it features native Google Sign-In authentication and seamless WebView integration.

---

## Documentation Structure

| File | Purpose | Status |
|------|---------|--------|
| **README.md** | Quick start guide and overview | Active |
| **PROJECT_STATUS.md** | Detailed project status and configuration | Active |
| **TESTING.md** | Testing guide and troubleshooting | Active |
| **DOCS_SUMMARY.md** | This file - Documentation overview | Active |
| **CLEANUP_REPORT.md** | Project cleanup history | Archive |

---

## Key Features

### Native Google Authentication
- Google Play Services SDK integration (v20.7.0)
- Native account picker UI
- Automatic click interception for login buttons
- JavaScript bridge for native/web communication
- Session persistence with Capacitor Preferences

### WebView Integration
- Loads web app: `https://app.my-coach-finder.com/go`
- Full-screen experience
- Hardware acceleration enabled
- HTTPS-only communication
- Custom User Agent configuration

### Push Notifications
- Firebase Cloud Messaging configured
- Device token registration ready
- Notification infrastructure in place

---

## Authentication Flow

```
1. User clicks "Continue with Google" button
2. JavaScript bridge intercepts click
3. Native Google Sign-In launches account picker
4. User selects Google account
5. ID token sent to backend: /auth/google/native?id_token=XXX
6. Backend validates and returns JWT
7. JWT stored in localStorage (key: 'token')
8. User navigated to authenticated page
```

---

## Technical Specifications

### App Configuration
- **Package ID:** `com.mycoachfinder.app`
- **App Name:** My Coach Finder
- **Version:** 1.0.0
- **Min SDK:** Android 5.1 (API 22)
- **Target SDK:** Android 14 (API 34)
- **APK Size:** 4.7MB (debug build)

### OAuth Configuration
- **Web Client ID:** `353309305721-ir55d3eiiucm5fda67gsn9gscd8eq146.apps.googleusercontent.com`
- **Android Client:** Auto-detected via package name
- **SHA-1 Fingerprint:** `B0:F8:1D:C6:AE:7B:D7:B9:0C:9F:5D:41:E0:A3:1A:DA:39:37:4A:D1`

### Tech Stack
| Component | Technology | Version |
|-----------|------------|---------|
| Framework | Capacitor | 6.x |
| Build Tool | Gradle | 8.2.1 |
| Runtime | Node.js | 20.19.5 |
| Java | OpenJDK | 17 |
| Android SDK | Command-line Tools | 34 |
| Auth Library | Google Play Services | 20.7.0 |

---

## Project Structure

```
andruid/
├── README.md                     # Quick start guide
├── PROJECT_STATUS.md             # Detailed status
├── DOCS_SUMMARY.md              # This file
├── TESTING.md                    # Testing guide
├── CLEANUP_REPORT.md            # Cleanup history
├── package.json                  # Dependencies
├── capacitor.config.json         # Configuration
├── www/                          # Web assets
│   └── index.html
└── android/                      # Native Android
    ├── app/
    │   ├── src/main/
    │   │   ├── java/com/mycoachfinder/app/
    │   │   │   └── MainActivity.java
    │   │   ├── AndroidManifest.xml
    │   │   └── res/
    │   └── build/outputs/apk/debug/
    │       └── app-debug.apk    # 4.7MB
    └── build.gradle
```

---

## Build & Installation

### Quick Build
```bash
# Sync and build
npx cap sync android
cd android && ./gradlew assembleDebug

# Install
adb install -r android/app/build/outputs/apk/debug/app-debug.apk
```

### For MIUI/Xiaomi Devices
```bash
# Push to Downloads folder
adb push android/app/build/outputs/apk/debug/app-debug.apk /sdcard/Download/MyCoachFinder.apk

# Then install from File Manager app
```

---

## Testing Status

### Working Features
- App launches successfully
- WebView loads web application
- Native Google Sign-In with account picker
- ID token retrieval and backend communication
- JWT storage and session persistence
- Login persists after app restart
- Logout functionality
- Navigation and page loading
- MIUI compatibility

### Known Issues
- None currently

---

## Common Commands

### Development
```bash
# Install dependencies
npm install

# Sync to Android
npx cap sync android

# Build APK
cd android && ./gradlew assembleDebug

# Install on device
adb install -r android/app/build/outputs/apk/debug/app-debug.apk
```

### Debugging
```bash
# View authentication logs
adb logcat | grep -E "(NativeAuth|Native Bridge)"

# Check devices
adb devices

# Clear app data
adb shell pm clear com.mycoachfinder.app

# Chrome DevTools
chrome://inspect (in Chrome browser)
```

---

## Troubleshooting

### Common Issues

**APK won't install**
- Solution: `adb uninstall com.mycoachfinder.app`

**Device not found**
- Solution: `adb kill-server && adb start-server`

**MIUI installation blocked**
- Solution: Use `adb push` method (see above)

**Google Sign-In fails**
- Solution: Clear Google Play Services cache
- Command: `adb shell pm clear com.google.android.gms`

---

## Future Roadmap

### Phase 1: Production Release
- Release build with signed APK
- Google Play Store submission
- Production testing

### Phase 2: Enhanced Features
- Refresh token system
- Biometric authentication
- Platform detection

### Phase 3: Analytics
- User behavior tracking
- Conversion analytics
- Performance monitoring

---

## Resources

### Documentation
- [Capacitor Docs](https://capacitorjs.com/docs)
- [Android Developer Guide](https://developer.android.com)
- [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging)

### Key Files
- APK: `android/app/build/outputs/apk/debug/app-debug.apk`
- Config: `capacitor.config.json`
- Manifest: `android/app/src/main/AndroidManifest.xml`
- Build: `android/app/build.gradle`

---

**Last Updated:** October 27, 2025
**Status:** Production Ready - Android Only
