# iOS Native Builds

The commands below are baseline examples.

Two build formats are used depending on the target:

| Format | Target | Preferred configuration |
|--------|--------|------------------------|
| `.app` | iOS Simulator | Debug |
| `.ipa` | Physical device | Release |

Before building, determine the scheme name:

```bash
xcodebuild -list
# or, if the project uses a workspace:
xcodebuild -list -workspace <Name>.xcworkspace
```

Use `Debug` for simulator builds — faster compilation, no provisioning needed. Use `Release` for physical device builds — tests real user flows with full optimization.

## Simulator build (.app)

Use `Debug` for simulator builds.

```bash
xcodebuild \
  -scheme <SchemeName> \
  -sdk iphonesimulator \
  -configuration Debug \
```

After building, apply an ad-hoc signature so the simulator runtime accepts the binary:

```bash
codesign -f -s - build/Build/Products/Debug-iphonesimulator/<SchemeName>.app
```

The `.app` bundle will be at `build/Build/Products/Debug-iphonesimulator/<SchemeName>.app`.

---

## Physical device build (.ipa)

### Step 1. Find your Team ID

```bash
# from the project settings (preferred)
xcrun xcodebuild -showBuildSettings -scheme <SchemeName> | grep DEVELOPMENT_TEAM

# or from the keychain (team ID is in parentheses after the certificate name)
security find-identity -v -p codesigning
```

### Step 2. Archive

```bash
xcodebuild archive \
  -scheme <SchemeName> \
  -configuration Release \
  -destination "generic/platform=iOS" \
  -archivePath build/<SchemeName>.xcarchive \
  -allowProvisioningUpdates \
  DEVELOPMENT_TEAM=<TeamID> \
  CODE_SIGN_STYLE=Automatic
```

### Step 3. Export to .ipa

`ExportOptions.plist` is required and cannot be passed inline. Generate it on the fly:

```bash
cat > /tmp/ExportOptions.plist << EOF
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
  <key>method</key>
  <string>development</string>
  <key>teamID</key>
  <string><TeamID></string>
</dict>
</plist>
EOF

xcodebuild -exportArchive \
  -archivePath build/<SchemeName>.xcarchive \
  -exportPath build/ipa/ \
  -exportOptionsPlist /tmp/ExportOptions.plist \
  -allowProvisioningUpdates
```

The `.ipa` will be at `build/ipa/<SchemeName>.ipa`.
