# React Native Builds

> `npx react-native run-ios` / `run-android` build, install **over** the existing app, and launch — they do **not** clear app data (login, onboarding, and cache carry over). To start from a clean state you must reset first: [Install iOS](install-ios.md#clean-install-reset-app-state) · [Install Android](install-android.md#clean-install-reset-app-state).

## Debug (with Metro)

Debug builds load JS from Metro bundler at runtime. Start Metro first:

```bash
npx react-native start
# or
npx expo start
```

### Option 1: xcodebuild / Gradle

- iOS → [builds-ios-native.md](builds-ios-native.md) (Debug configuration)
- Android → [builds-android.md](builds-android.md) (assembleDebug)

### Option 2: npx react-native (build + install + launch in one step)

```bash
# iOS
npx react-native run-ios --udid <simulator-udid> --mode Debug

# Android
npx react-native run-android --device <adb-device-id> --mode debug
```

---

## Release

JS bundle is compiled into the binary — no Metro needed. Use for testing real user flows.

### Option 1: npx react-native (build + install + launch in one step)

```bash
# iOS
npx react-native run-ios --udid <simulator-udid> --mode Release

# Android
npx react-native run-android --device <adb-device-id> --mode release
```

### Option 2: xcodebuild / Gradle

- iOS → [builds-ios-native.md](builds-ios-native.md) (Release configuration)
- Android → [builds-android.md](builds-android.md) (assembleRelease)
