# Workflow: Run Saved Tests with the noqa Agent

Runs **already-saved** test cases (by number) autonomously on a connected device using the noqa agent. Requires a paid account with credits.

> Developing a new scenario? Drive it on the device first by describing the target elements in plain language and iterate before saving the case — see [Debug directly on device](debug-on-device.md).

> References: [Devices](../references/devices.md) · [Run Tests](../references/run.md) · [Builds](../references/builds.md) · [iOS Native Builds](../references/builds-ios-native.md) · [React Native Builds](../references/builds-react-native.md) · [Android Builds](../references/builds-android.md)

## Steps

### 1. Connect a device

List available devices and connect the one to test on. See [Devices](../references/devices.md).

### 2. Provide a build

**Always run saved cases with a build.** Each run reinstalls the app from the build, which keeps a regression suite consistent — never rely on whatever happens to be installed on the device (e.g. a React Native app served via Metro is not reinstalled between runs). Produce a binary if needed — [iOS Native Builds](../references/builds-ios-native.md), [React Native Builds](../references/builds-react-native.md), or [Android Builds](../references/builds-android.md); use a Release build unless the user explicitly wants a Debug build.

> **If the user wants to skip the build and run against the app already installed on the device, confirm with them first and warn about the consequence.** Without a reinstall the app is not reset — leftover login, completed onboarding, and data from previous runs or manual use carry over. Test cases assume a [cold launch from a clean state](../concepts/writing-test-cases.md#every-test-starts-from-a-cold-launch), so a case can pass or fail for the wrong reason. It is worse with several cases in one run: state from one test bleeds into the next. Acceptable only for a quick single-case check; for a regression suite, insist on a build. If they still want to reuse the installed app, reset its state first — [Install iOS](../references/install-ios.md#clean-install-reset-app-state) · [Install Android](../references/install-android.md#clean-install-reset-app-state).

Pass the build one of two ways:

**Option A — register the build first, then run by ID (recommended):**

Register it once with `noqa builds create` to get a build ID (see [Builds](../references/builds.md)), then run the cases with `--build-id` per [Run Tests](../references/run.md). Reuse the same ID across runs instead of re-passing a path.

**Option B — pass a build file path inline:**

For a one-off, skip the registry and run the cases with `--build-path <absolute path>` per [Run Tests](../references/run.md).

### 3. Run the tests

Run the saved case numbers with the run command in [Run Tests](../references/run.md). A `run_id` is returned — tell the user the run has started and share the ID. Inspect pass/fail per step with `noqa runs get` (see [Run Tests](../references/run.md)).

## Notes

- All case numbers must belong to the same app (the prefix you pass)
- The active device must be connected before running
- Supported build formats: `.ipa` (iOS device), `.app` (iOS simulator), `.apk` (Android)
- If the run fails with "Insufficient credits", tell the user to add credits or subscribe at https://noqa.ai
