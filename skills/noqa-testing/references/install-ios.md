# Installing an iOS Build

> Only needed for **debug mode** (manual device interaction). When running tests via the noqa agent, the build is installed automatically.

The commands below are baseline examples and may need adjustment depending on the Xcode version and device configuration.

## Simulator (.app)

```bash
xcrun simctl install <device-id> <path-to.app>
```

Get `<device-id>` from `noqa devices ios`.

## Physical device (.ipa)

Requires Xcode 15+:

```bash
xcrun devicectl device install app --device <device-id> <path-to.ipa>
```

Get `<device-id>` from `noqa devices ios`.

## Clean install (reset app state)

A plain install over an existing app keeps its data — login, completed onboarding, cache. To start from a truly clean state, remove the app (with its data) first, then install.

### Simulator (.app)

```bash
# wipe just this app + its data container, then reinstall
xcrun simctl uninstall <device-id> <bundle-id>
xcrun simctl install <device-id> <path-to.app>

# or wipe the entire simulator (all apps) — the device must be shut down first
xcrun simctl erase <device-id>
```

### Physical device (.ipa)

```bash
xcrun devicectl device uninstall app --device <device-id> <bundle-id>
xcrun devicectl device install app --device <device-id> <path-to.ipa>
```

Get `<bundle-id>` from the build's Info.plist. The location differs by build type:

```bash
# .app (simulator build): Info.plist sits at the bundle root
/usr/libexec/PlistBuddy -c "Print :CFBundleIdentifier" <path-to.app>/Info.plist

# .ipa (device build): an .ipa is a zip — Info.plist lives inside Payload/*.app
unzip -p <path-to.ipa> 'Payload/*.app/Info.plist' | plutil -extract CFBundleIdentifier raw -o - -
```
