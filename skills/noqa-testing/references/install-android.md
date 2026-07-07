# Installing an Android Build

> Only needed for **debug mode** (manual device interaction). When running tests via the noqa agent, the build is installed automatically.

The commands below are baseline examples and may need adjustment depending on the connected device configuration.

## Physical device or emulator (.apk)

Get the device ID from `noqa devices android`. If multiple devices are connected, specify the target:

```bash
adb -s <adb-device-id> install <path-to.apk>
```

## Clean install (reset app state)

`adb install -r` — and `npx react-native run-android`, which uses it — reinstalls but **keeps** the app's data (login, completed onboarding, cache). To reset state, clear the data or uninstall first.

```bash
# clear app data, keep the app installed
adb -s <adb-device-id> shell pm clear <package-name>

# or fully uninstall, then install fresh
adb -s <adb-device-id> uninstall <package-name>
adb -s <adb-device-id> install <path-to.apk>
```

Get `<package-name>` from `applicationId` in `app/build.gradle`, or list installed packages with `adb -s <adb-device-id> shell pm list packages`.
