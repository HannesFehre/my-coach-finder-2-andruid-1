# Credentials Setup Guide

This guide explains how to set up the necessary credential files for building the My Coach Finder Android app.

## Required Files

The following files contain sensitive data and are **NOT** committed to GitHub:

1. `gradle.properties` - Contains keystore passwords
2. `app/google-services.json` - Contains Firebase configuration
3. `../andruid_privat/app.jks` - The signing keystore file

## Setup Instructions

### 1. Gradle Properties (gradle.properties)

Copy the template file and add your keystore passwords:

```bash
cd android
cp gradle.properties.template gradle.properties
```

Edit `gradle.properties` and replace:
- `YOUR_PASSWORD_HERE` with your actual keystore password (both lines)

### 2. Firebase Configuration (google-services.json)

Download from Firebase Console:

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Select project: **my-caoch-finder-2**
3. Go to **Project Settings** > **General**
4. Scroll to **Your apps** section
5. Find the Android app (package: `app.mycoachfinder.app`)
6. Click **google-services.json** download button
7. Save to: `android/app/google-services.json`

Or use the template:
```bash
cd android/app
cp google-services.json.template google-services.json
```

Then edit and replace all placeholder values.

### 3. Keystore File

The app is signed with a keystore located at:
- `../andruid_privat/app.jks`

This file should be stored securely outside the repository.

**Keystore details:**
- Alias: `key0`
- Password: Set in `gradle.properties`

## Building the App

Once all credential files are in place:

```bash
cd android

# Debug build
./gradlew assembleDebug

# Release build (requires all credentials)
export KEYSTORE_PASSWORD="your_password"
export KEY_PASSWORD="your_password"
./gradlew bundleRelease
```

## Security Notes

- **NEVER** commit `gradle.properties` with actual passwords
- **NEVER** commit `google-services.json` with real API keys
- **NEVER** commit keystore files (`.jks`, `.keystore`)
- These files are protected by `.gitignore`
- Always use environment variables or local files for sensitive data

## Verification

Check that sensitive files are ignored:

```bash
git status
# Should NOT show:
# - gradle.properties
# - app/google-services.json
# - Any .jks or .keystore files
```
