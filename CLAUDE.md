# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
pnpm start          # Start Expo dev server
pnpm ios            # Run on iOS simulator
pnpm android        # Run on Android emulator/device
pnpm web            # Run web version
```

No test runner or linter is configured in this project.

## Stack

- **React Native 0.81.5 / Expo SDK 54** with New Architecture enabled
- **TypeScript** with strict mode; path alias `@/*` → `./src/*`
- **React Navigation 7** using the static navigation API (`createStaticNavigation`)
- **expo-camera** for camera access and barcode/QR scanning
- **react-native-background-geolocation 5.x** for persistent GPS tracking (requires a JWT license key in `app.json` → `TSLocationManagerLicense`)

## Architecture

### Navigation

Uses the **static navigation** approach introduced in React Navigation 7 — navigation trees are declared as plain objects rather than JSX, then passed to `createStaticNavigation`.

```
RootStack (NativeStack)
  └── BottomTab
        ├── Camera
        └── Geo
```

`src/navigation/navigation.tsx` → `createStaticNavigation(RootStack)` → exported as `<Navigation />` rendered inside `App.tsx`.

### Screen structure

Each screen lives in `src/screens/<ScreenName>/` with its own `components/` subdirectory. Screens are barrel-exported from `src/screens/index.ts`.

### Key conventions

- `applyOpacity` in `src/utils/pressablesFeedback.ts` is a higher-order function used to apply press feedback styles to `Pressable` components — use it instead of raw opacity state.
- `simpleAlert` in `src/utils/alerts.ts` wraps `Alert.alert` with a Spanish close button ("Cerrar") — use this app-wide for alerts.
- `FilledButton` in `src/components/button.tsx` is the standard button component.

### Background Geolocation

Configuration and helper utilities live in `src/screens/geoScreen/helper.ts`. The plugin requires specific Gradle and iOS plugin entries already set up in `app.json`. Location data is stored in the plugin's internal SQLite database; `GeoScreen.tsx` exposes start/stop, view, share (exports as a `.txt` file via `expo-sharing`), and clear database controls.

### App entry point

`index.ts` → `registerRootComponent(App)`. `App.tsx` simply renders `<Navigation />` inside a `SafeAreaProvider`.
