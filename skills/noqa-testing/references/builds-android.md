# Android Builds

The commands below are baseline examples. Project-specific settings (module name, build variants, signing config) may differ — inspect `build.gradle` and `settings.gradle` first and adjust accordingly.

Android projects use Gradle to build `.apk` files. The build script is either `./gradlew` (Unix) or `gradlew.bat` (Windows).

`assembleDebug` vs `assembleRelease`: use `Release` for testing real user flows. Use `Debug` only during active development. For React Native, `Debug` requires Metro bundler running — without it the app shows a connection error.

Before building, determine the available build variants:

```bash
./gradlew tasks --group build
# or list all variants:
./gradlew app:tasks --all | grep assemble
```

---

## Build (.apk)

### Debug

No signing required — works out of the box.

```bash
./gradlew assembleDebug
```

The `.apk` will be at `app/build/outputs/apk/debug/app-debug.apk`.

---

### Release

Requires a keystore for signing. Check if one is already configured in the project:

```bash
cat app/build.gradle | grep -A 10 "signingConfigs"
# or for Kotlin DSL:
cat app/build.gradle.kts | grep -A 10 "signingConfigs"
```

Find the keystore file:

```bash
find . -name "*.keystore" -o -name "*.jks" 2>/dev/null
```

If `signingConfigs` already has a `release` block in `build.gradle`, just run:

```bash
./gradlew assembleRelease
```

Otherwise pass signing properties directly:

```bash
./gradlew assembleRelease \
  -Pandroid.injected.signing.store.file=<path-to.keystore> \
  -Pandroid.injected.signing.store.password=<store-password> \
  -Pandroid.injected.signing.key.alias=<key-alias> \
  -Pandroid.injected.signing.key.password=<key-password>
```

The `.apk` will be at `app/build/outputs/apk/release/app-release.apk`.

If the project has multiple modules, replace `app` with the correct module name (check `settings.gradle`).
