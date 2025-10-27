# Testing My Coach Finder Android App

**Last Updated:** October 27, 2025
**Platform:** Android Only
**APK Size:** 4.7MB (debug)

---

## APK Location

```
/home/liz/Desktop/Module/MyCoachFinder/app/andruid/android/app/build/outputs/apk/debug/app-debug.apk
```

---

## Prerequisites

### 1. Enable USB Debugging on Android Device

1. Go to **Settings** → **About Phone**
2. Tap **Build Number** 7 times to enable Developer Mode
3. Go back to **Settings** → **Developer Options**
4. Enable **USB Debugging**
5. Enable **Install via USB** (if available)

### 2. Connect Your Device

1. Connect Android phone to Linux PC via USB
2. On phone: Tap **Allow** on "Allow USB debugging?" prompt
3. Check "Always allow from this computer" for convenience

### 3. Verify Connection

```bash
export ANDROID_HOME=~/android-sdk
export PATH=$PATH:$ANDROID_HOME/platform-tools
adb devices
```

Expected output:
```
List of devices attached
ABC123XYZ    device
```

---

## Installation Methods

### Method 1: Install via ADB (Recommended)

```bash
cd /home/liz/Desktop/Module/MyCoachFinder/app/andruid
adb install -r android/app/build/outputs/apk/debug/app-debug.apk
```

Expected output: `Success`

### Method 2: For MIUI/Xiaomi Devices

USB installation may be blocked by MIUI security:

```bash
# Push APK to Downloads folder
adb push android/app/build/outputs/apk/debug/app-debug.apk /sdcard/Download/MyCoachFinder.apk

# Then on phone:
# Open File Manager → Downloads → Tap MyCoachFinder.apk → Install
```

### Method 3: Manual Transfer

1. Copy APK to phone via USB/Email/Cloud
2. Open the APK file on phone
3. Tap **Install**

---

## Testing Checklist

### Basic Functionality
- [ ] App launches without crashing
- [ ] Web app loads: `https://app.my-coach-finder.com/go`
- [ ] No blank screen or errors
- [ ] Navigation works correctly
- [ ] Back button functions properly

### Native Google Authentication
- [ ] Click "Continue with Google" button
- [ ] Native account picker appears
- [ ] Select Google account
- [ ] Authentication completes successfully
- [ ] Redirected to home page after login
- [ ] Session persists after closing app
- [ ] Session persists after app restart
- [ ] Logout works correctly

### Performance
- [ ] Pages load quickly
- [ ] Scrolling is smooth
- [ ] No lag or stuttering
- [ ] Acceptable battery usage

### Data Storage
- [ ] Login state persists after restart
- [ ] localStorage works correctly
- [ ] Session tokens maintained

---

## Common Issues & Solutions

### "Installation blocked"
**Solution:** Enable "Install from Unknown Sources"
- Settings → Security → Unknown Sources → Enable

### "App not installed"
**Solution:** Uninstall old version first
```bash
adb uninstall com.mycoachfinder.app
adb install android/app/build/outputs/apk/debug/app-debug.apk
```

### "Device unauthorized"
**Solution:**
1. Revoke USB debugging authorizations on phone
2. Disconnect and reconnect USB
3. Accept USB debugging prompt

### "ADB device not found"
```bash
adb kill-server
adb start-server
adb devices
```

### Blank white screen
1. Check internet connection
2. Verify `https://app.my-coach-finder.com/go` loads in browser
3. Check logs: `adb logcat | grep -i "capacitor\|error"`

### Google Sign-In fails
**Solution:** Clear Google Play Services cache
```bash
adb shell pm clear com.google.android.gms
```

---

## Debugging Tools

### View Real-time Logs
```bash
# All logs
adb logcat

# Filter for authentication
adb logcat | grep -E "(NativeAuth|Native Bridge)"

# Filter for Capacitor
adb logcat | grep Capacitor

# Filter for errors
adb logcat | grep -E "ERROR|FATAL"

# Clear and view fresh logs
adb logcat -c && adb logcat
```

### Chrome DevTools Remote Debugging
1. Open app on phone
2. On PC: Open Chrome and go to `chrome://inspect`
3. Click **Inspect** on the WebView
4. Debug HTML/CSS/JavaScript
5. View console logs
6. Monitor network requests

### Clear App Data
```bash
adb shell pm clear com.mycoachfinder.app
```

### Uninstall App
```bash
adb uninstall com.mycoachfinder.app
```

### Take Screenshot
```bash
adb shell screencap /sdcard/screenshot.png
adb pull /sdcard/screenshot.png ~/Desktop/
```

### Screen Recording
```bash
adb shell screenrecord /sdcard/demo.mp4 --time-limit 60
# Stop with Ctrl+C
adb pull /sdcard/demo.mp4 ~/Desktop/
```

---

## Performance Monitoring

### Memory Usage
```bash
adb shell dumpsys meminfo com.mycoachfinder.app
```

### Battery Usage
```bash
adb shell dumpsys batterystats com.mycoachfinder.app
```

### Network Activity
```bash
adb shell dumpsys netstats com.mycoachfinder.app
```

---

## Rebuilding After Changes

```bash
# Sync changes to Android
npx cap sync android

# Rebuild APK
cd android && ./gradlew assembleDebug

# Reinstall on device
adb install -r android/app/build/outputs/apk/debug/app-debug.apk
```

---

## Authentication Flow Testing

### Expected Flow
```
1. User opens app → Login page loads
2. Click "Continue with Google" button
3. Native account picker appears (NOT web OAuth)
4. Select Google account
5. ID token retrieved automatically
6. Token sent to backend: /auth/google/native?id_token=XXX
7. Backend returns JWT
8. JWT stored in localStorage (key: 'token')
9. User redirected to home page
10. Session persists after app restart
```

### Success Indicators in Logs
```bash
adb logcat | grep -E "(NativeAuth|Native Bridge)"
```

Look for:
- `[Native Bridge] Injecting native auth`
- `[Native Bridge] Intercepted Google sign-in`
- `NativeAuth: Starting Google Sign-In`
- `NativeAuth: Sign-In successful`
- `[Native Bridge] Backend response status: 200`

---

## Test Report Template

```markdown
# Test Report - My Coach Finder Android

**Date:** [Date]
**Tester:** [Name]
**Device:** [Phone Model]
**Android Version:** [Version]
**APK Version:** 1.0.0 (debug)

## Test Results

### ✅ Passed
- App launches successfully
- Native Google Sign-In works
- Session persistence works
- [Add more...]

### ❌ Failed
- [List any failures with details]

### ⚠️ Minor Issues
- [List minor issues]

## Screenshots
[Attach screenshots]

## Recommendations
[Suggestions for improvements]
```

---

## Building Release APK

For production/Play Store:

```bash
# Build release APK
cd android && ./gradlew assembleRelease

# Output: android/app/build/outputs/apk/release/app-release-unsigned.apk
```

For Play Store submission:
1. Generate signing key
2. Sign the APK
3. Use Android App Bundle (AAB) format
4. See Google Play documentation

---

## Next Steps

After testing is complete:

1. **Production Release** - Build signed APK for Play Store
2. **Firebase Setup** - Enable push notifications
3. **Enhanced Auth** - Implement refresh tokens
4. **Analytics** - Track app vs web usage
5. **Beta Testing** - Share with select users
6. **Play Store** - Submit to Google Play

---

## Support

For issues:
1. Check logs: `adb logcat`
2. Verify internet connectivity
3. Ensure web app URL is accessible
4. Review `AndroidManifest.xml` permissions
5. Check `capacitor.config.json` configuration

**Capacitor Docs:** https://capacitorjs.com/docs/debugging

---

**Status:** Ready for Testing
