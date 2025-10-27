# My Coach Finder - Android App

> Native Android application for My Coach Finder platform - Connect with your coach, achieve your goals.

**Platform:** Android 5.1+ (API 22+) | **Technology:** Capacitor 6.x WebView + Native Google Sign-In | **Status:** Production Ready

---

## About My Coach Finder

**My Coach Finder** is Germany's leading platform connecting individuals with qualified coaches across diverse specialties - from life coaching and career planning to health, relationships, mindfulness, and financial guidance.

- **1,000+ Coaches** across various specialties
- **4.8/5 Rating** from 268+ reviews
- **Free for Seekers** - No cost to browse and connect
- **24/7 Support** - Multilingual customer service
- **GDPR Compliant** - German data protection standards

**Website:** https://my-coach-finder.de | **Web App:** https://app.my-coach-finder.com/go

---

## Quick Start

### Install on Your Android Device

```bash
# Connect device via USB and enable USB Debugging
cd /home/liz/Desktop/Module/MyCoachFinder/app/andruid

# Install the app
adb install android/app/build/outputs/apk/debug/app-debug.apk
```

### Build from Source

```bash
# Install dependencies
npm install

# Sync web assets to Android
npx cap sync android

# Build APK
cd android && ./gradlew assembleDebug

# APK output: android/app/build/outputs/apk/debug/app-debug.apk
```

---

## Key Features

### Native Google Authentication
- **Account Picker UI** - Select from device Google accounts
- **Automatic Click Interception** - Detects "Continue with Google" buttons
- **JavaScript Bridge** - Seamless native/web integration
- **Session Persistence** - Login persists after app restart
- **Production Tested** - Fully working authentication flow

### WebView Wrapper
- Loads web app: `https://app.my-coach-finder.com/go`
- Full-screen experience
- Hardware acceleration enabled
- HTTPS-only communication

### Push Notifications
- Firebase Cloud Messaging configured
- Device token registration ready
- Notification delivery infrastructure

---

## Authentication Flow

```
User clicks "Continue with Google" button
    ↓
JavaScript bridge intercepts the click
    ↓
Native Google Sign-In SDK launches account picker
    ↓
User selects Google account
    ↓
ID token sent to backend: /auth/google/native?id_token=XXX
    ↓
Backend validates token and returns JWT
    ↓
JWT stored in localStorage (key: 'token')
    ↓
User navigated to authenticated page
```

---

## Configuration

### App Identity
- **Package ID:** `com.mycoachfinder.app`
- **App Name:** My Coach Finder
- **Version:** 1.0.0
- **Min SDK:** Android 5.1 (API 22)
- **Target SDK:** Android 14 (API 34)

### OAuth Configuration
- **Web Client ID:** `353309305721-ir55d3eiiucm5fda67gsn9gscd8eq146.apps.googleusercontent.com`
- **Android Client:** Auto-detected via package name
- **SHA-1 Fingerprint:** `B0:F8:1D:C6:AE:7B:D7:B9:0C:9F:5D:41:E0:A3:1A:DA:39:37:4A:D1`

---

## Project Structure

```
andruid/
├── README.md                     # This file
├── PROJECT_STATUS.md             # Detailed project status
├── DOCS_SUMMARY.md              # Documentation summary
├── TESTING.md                    # Testing guide
├── package.json                  # Node.js dependencies
├── capacitor.config.json         # Capacitor configuration
├── www/                          # Web assets (minimal)
│   └── index.html               # Redirects to web app
└── android/                      # Native Android project
    ├── app/
    │   ├── src/main/
    │   │   ├── java/com/mycoachfinder/app/
    │   │   │   └── MainActivity.java         # Native Google Sign-In
    │   │   ├── AndroidManifest.xml          # Permissions & config
    │   │   └── res/                         # Resources
    │   └── build/outputs/apk/debug/
    │       └── app-debug.apk                # Installable APK (4.7MB)
    └── build.gradle                         # Build configuration
```

---

## Tech Stack

| Component | Technology | Version |
|-----------|------------|---------|
| Framework | Capacitor | 6.x |
| Build Tool | Gradle | 8.2.1 |
| Runtime | Node.js | 20.19.5 |
| Java | OpenJDK | 17 |
| Android SDK | Command-line Tools | 34 |
| Auth Library | Google Play Services | 20.7.0 |

---

## Common Commands

### Development
```bash
# Rebuild after changes
npx cap sync android
cd android && ./gradlew assembleDebug

# Install on device
adb install -r android/app/build/outputs/apk/debug/app-debug.apk

# For MIUI/Xiaomi devices (USB install blocked)
adb push android/app/build/outputs/apk/debug/app-debug.apk /sdcard/Download/MyCoachFinder.apk
# Then install from File Manager app
```

### Debugging
```bash
# View authentication logs
adb logcat | grep -E "(NativeAuth|Native Bridge)"

# Chrome DevTools
# 1. Open app on phone
# 2. In Chrome: chrome://inspect
# 3. Click "Inspect" on WebView

# Clear app data
adb shell pm clear com.mycoachfinder.app
```

---

## Troubleshooting

### Common Issues

| Issue | Solution |
|-------|----------|
| APK won't install | `adb uninstall com.mycoachfinder.app` |
| Device not found | `adb kill-server && adb start-server` |
| Blank screen | Check internet connection |
| MIUI install blocked | Use `adb push` method (see above) |
| Google Sign-In fails | Clear Google Play Services: `adb shell pm clear com.google.android.gms` |

---

## Documentation

- **[README.md](README.md)** - This file (Quick start & overview)
- **[PROJECT_STATUS.md](PROJECT_STATUS.md)** - Detailed project status
- **[DOCS_SUMMARY.md](DOCS_SUMMARY.md)** - Documentation summary
- **[TESTING.md](TESTING.md)** - Testing guide & checklist
- **[CLEANUP_REPORT.md](CLEANUP_REPORT.md)** - Cleanup history

---

## License

Proprietary - My Coach Finder Platform

---

**Built with Capacitor** ⚡ by Ionic | **Last Updated:** October 27, 2025
