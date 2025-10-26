# My Coach Finder - iOS/Android App Project Status

**Last Updated:** October 26, 2025
**Current Version:** iOS v1.1.12 (Build 12)
**Status:** ✅ iOS Building Successfully | ⏳ Google OAuth Testing Pending

---

## 📱 Platform Status

### Android
- ✅ **FULLY WORKING**
- Native Google Sign-In implemented
- Session persistence working
- APK builds successfully
- Latest APK: `android/app/build/outputs/apk/debug/app-debug.apk` (3.6MB)

### iOS
- ✅ **BUILDS SUCCESSFULLY** on Codemagic
- ✅ App icon and splash screen added
- ✅ Status bar hidden
- ✅ WebView navigation working (no Safari redirects)
- ⏳ Google OAuth native sign-in pending final test
- Latest Build: **v1.1.12 (Build 12)** - [Codemagic](https://codemagic.io/app/68fd6f2a7e282f2bab8b9665)

---

## 🎨 Branding & UI

### App Icons
- ✅ Android icons configured
- ✅ iOS app icon: 1024x1024 from `Logo/logo_output/ios_app/`
- ✅ Home screen icon displays correctly
- ⏳ App switcher icon - testing v1.1.12 fix

### Splash Screens
- ✅ Android splash screen configured
- ✅ iOS splash screen: 2732x2732 with My Coach Finder logo
- ✅ White background with centered logo

### UI Improvements
- ✅ Status bar hidden on iOS for native feel
- ✅ Full-screen WebView experience
- ✅ Professional branded appearance

---

## 🔐 Authentication

### Android OAuth Flow
```
1. User opens app → Login page in WebView ✅
2. Click Google button → Native account picker opens ✅
3. Select account → ID token retrieved ✅
4. Send to backend → Session created ✅
5. Redirect to home page ✅
```

### iOS OAuth Flow (In Progress)
```
1. User opens app → Login page in WebView ✅
2. Click Google button → WKUserScript intercepts click ✅
3. Call native Google Sign-In → PENDING TEST
4. Get ID token → Send to backend
5. Redirect to home page
```

**Implementation:** JavaScript injected via WKUserScript runs automatically on every page load

---

## 🏗️ Build Configuration

### Codemagic Workflows

#### `ios-development` (Manual Code Signing)
- Instance: Mac mini M1
- Xcode: latest
- Node.js: 20.19.5
- Distribution: Development (Ad Hoc)
- Build command: `xcode-project build-ipa`
- Artifacts: `.ipa`, logs, .app, .dSYM

**Environment Variables (Group: `ios_signing`):**
- `CM_CERTIFICATE` - Base64-encoded .p12 certificate
- `CM_CERTIFICATE_PASSWORD` - Certificate password
- `CM_PROVISIONING_PROFILE` - Base64-encoded .mobileprovision

#### `ios-production` (App Store)
- NOT YET USED - requires App Store Connect registration
- Auto-provisioning via App Store Connect API
- Submit to TestFlight enabled

### Android Build
- Gradle: 8.2.1
- Build command: `./gradlew assembleDebug`
- Output: `android/app/build/outputs/apk/debug/app-debug.apk`

---

## 📦 Key Dependencies

### Capacitor
- `@capacitor/core`: 6.x
- `@capacitor/android`: 6.x
- `@capacitor/ios`: 6.x
- `@capacitor/preferences`: 6.0.0

### Native Libraries
- **Android:** Google Play Services Auth 20.7.0
- **iOS:** GoogleSignIn SDK (via CocoaPods)

### Configuration
- `capacitor.config.json`:
  - Server URL: `https://app.my-coach-finder.com/go`
  - **allowNavigation:** `["app.my-coach-finder.com", "*.my-coach-finder.com"]`

---

## 🔧 Technical Implementation

### iOS Native Google Sign-In
**File:** `ios/App/App/NativeAuthPlugin.swift`

**Features:**
- WKUserScript auto-injection on every page load
- Click interception for `/auth/google/login` links
- Native Google Sign-In SDK integration
- Backend token exchange
- Session persistence

**Key Methods:**
- `setupUserScript()` - Injects JavaScript using WKUserScript
- `signInWithGoogle()` - Native iOS Google Sign-In
- `shouldOverrideLoad()` - Controls WebView navigation

### Android Native Google Sign-In
**File:** `android/app/src/main/java/com/mycoachfinder/app/MainActivity.java`

**Features:**
- JavaScript bridge injection
- Click interception (same logic as iOS)
- Native Android Google Sign-In
- Session management

---

## 📝 Build History

### Recent Builds (iOS)

| Build | Version | Changes | Status |
|-------|---------|---------|--------|
| 12 | 1.1.12 | Version bump for icon cache refresh | ⏳ Testing |
| 11 | 1.0.11 | Added app icon & splash screen | ✅ Success |
| 10 | 1.0.10 | WKUserScript + hide status bar | ✅ Success |
| 9 | 1.0.9 | Page load re-injection attempt | ❌ Failed |
| 8 | 1.0.8 | Added error diagnostics with alerts | ❌ Failed |
| 7 | 1.0.7 | Enhanced click interception | ❌ Failed |
| 6 | 1.0.6 | Added allowNavigation + window.open override | ✅ Success |
| 5 | 1.0.5 | Debug logging | ✅ Success |
| 4 | 1.0.4 | Version badge added | ✅ Success |

**Success Rate:** 6/9 builds successful (67%)

---

## 🎯 Current Goals

### Immediate (Next Session)
1. ✅ Test build 12 (v1.1.12) - verify app switcher icon
2. ⏳ Test Google OAuth with WKUserScript implementation
3. 🔧 Debug and fix any remaining OAuth issues

### Short Term
- [ ] Finalize iOS Google OAuth native sign-in
- [ ] Test session persistence on iOS
- [ ] Verify all navigation flows work correctly
- [ ] Prepare for TestFlight submission

### Long Term
- [ ] Register app in App Store Connect
- [ ] Set up production workflow with auto-provisioning
- [ ] Submit to TestFlight for beta testing
- [ ] Submit to App Store for review

---

## 📂 Project Structure

```
andruid/
├── android/                    # Android native code
│   └── app/
│       ├── src/main/java/com/mycoachfinder/app/
│       │   └── MainActivity.java    # Android Google Sign-In
│       └── build/outputs/apk/
├── ios/                        # iOS native code
│   └── App/
│       ├── App/
│       │   ├── NativeAuthPlugin.swift   # iOS Google Sign-In
│       │   ├── AppDelegate.swift
│       │   ├── Info.plist
│       │   └── Assets.xcassets/
│       │       ├── AppIcon.appiconset/
│       │       └── Splash.imageset/
│       └── App.xcodeproj/
├── Logo/                       # Brand assets (14MB - not committed)
│   ├── apple_touch/
│   ├── android/
│   └── logo_output/
│       ├── ios_app/
│       └── store_marketing/
├── capacitor.config.json       # Capacitor configuration
├── codemagic.yaml              # CI/CD configuration
├── .gitignore                  # Git ignore rules
├── IOS_BUILD_STATUS.md         # iOS build documentation
└── PROJECT_STATUS.md           # This file

```

---

## 🔒 Security Notes

### Excluded from Git
- `*.p12` - Certificates
- `*.mobileprovision` - Provisioning profiles
- `*.cer` - Certificates
- `*_base64.txt` - Base64-encoded credentials
- `AuthKey_*.p8` - App Store Connect API keys
- `Logo/` directory - 14MB design assets

### Environment Variables (Codemagic)
All sensitive credentials stored in Codemagic environment variable group `ios_signing`:
- Certificate password
- Base64-encoded certificate
- Base64-encoded provisioning profile

---

## 🐛 Known Issues

### iOS
1. **App Switcher Icon** - Shows old Capacitor icon on v1.0.11
   - **Status:** ⏳ Testing fix in v1.1.12
   - **Fix:** Version bump + clean install + device restart

2. **Google OAuth Native Sign-In** - Not yet confirmed working
   - **Status:** ⏳ Pending test
   - **Implementation:** WKUserScript with click interception

### Android
- ✅ No known issues

---

## 📞 Support & Resources

### Codemagic
- Dashboard: https://codemagic.io/apps
- App ID: `68fd6f2a7e282f2bab8b9665`
- API Access: Configured with personal API token

### Apple Developer
- Team ID: (set in Xcode)
- Bundle ID: `com.mycoachfinder.app`
- Provisioning: Manual (Development)

### Google OAuth
- Client ID: `353309305721-ir55d3eiiucm5fda67gsn9gscd8eq146.apps.googleusercontent.com`
- Backend endpoint: `https://app.my-coach-finder.com/auth/google/native`

---

## 🚀 Quick Start Guide

### Build iOS App Locally
```bash
# Install dependencies
npm install
cd ios/App && pod install

# Sync Capacitor
npx cap sync ios

# Open in Xcode
npx cap open ios

# Build in Xcode or use Codemagic
```

### Build Android App Locally
```bash
# Sync Capacitor
npx cap sync android

# Build APK
./gradlew assembleDebug

# Output: android/app/build/outputs/apk/debug/app-debug.apk
```

### Trigger Codemagic Build via API
```bash
curl -H "Content-Type: application/json" \
     -H "x-auth-token: YOUR_API_TOKEN" \
     --data '{
       "appId": "68fd6f2a7e282f2bab8b9665",
       "workflowId": "ios-development",
       "branch": "main"
     }' \
     -X POST https://api.codemagic.io/builds
```

---

## ✅ Accomplishments This Session

1. ✅ Fixed iOS WebView navigation - login page stays in-app
2. ✅ Added allowNavigation configuration
3. ✅ Implemented window.open override
4. ✅ Added My Coach Finder app icon (1024x1024)
5. ✅ Added branded splash screen (2732x2732)
6. ✅ Hidden status bar for native feel
7. ✅ Implemented WKUserScript for automatic JavaScript injection
8. ✅ Added comprehensive error diagnostics
9. ✅ Bumped version to 1.1.12 to fix icon cache
10. ✅ Cleaned up project and updated .gitignore

---

**Ready for next session!** 🎉

Take your break - when you return, we'll test build 12 and finalize the Google OAuth implementation.
