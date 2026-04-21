# Features POC

A React Native proof of concept built with Expo SDK 54, exploring two capabilities: camera-based scanning and background geolocation tracking.

> **Note:** This is a POC. The code is intentionally minimal and is not optimized or hardened for production use.

## What it does

### Camera scanning
Provides QR code and barcode scanning via `expo-camera`. Camera permissions are requested at runtime. A single modal handles both scan types from the main camera screen.

### Background geolocation
Tracks device location in the background using `react-native-background-geolocation`. Locations are recorded every 10 meters of movement and persisted locally. From the geo screen, testers can:

- Start / stop tracking
- Print the recorded session (shown as an in-app alert)
- Share the session as a `.txt` file
- Clear the location database

## Running locally

```bash
pnpm start       # Expo dev server
pnpm ios         # iOS simulator
pnpm android     # Android emulator
```

## Building a test APK

The geolocation library requires `BuildConfig.DEBUG = true` to run without a commercial license. The `preview` EAS profile builds a debug-variant APK with the JS bundle embedded — no Metro server needed.

```bash
eas build --profile preview --platform android
```

## Tech stack

- Expo SDK 54 (New Architecture)
- `react-native-background-geolocation` (Transistor Software)
- `expo-camera`
- `@react-navigation/native`
