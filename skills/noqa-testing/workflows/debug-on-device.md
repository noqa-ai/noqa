# Workflow: Debug Directly on Device

You drive the device yourself — inspect, then tap/swipe/drag/type. **Prefer describing the target element in plain language** — noqa resolves its on-screen location for you (grounding — free, only requires being signed in). See [Actions by description](../references/actions-grounding.md). As a fallback, you can pass explicit relative coordinates read from the screen tree — see [Actions by coordinates](../references/actions-coordinates.md).

### 1. Connect a device
List available devices and connect the one to test on. See [Devices](../references/devices.md).

### 2. Make sure the app is on the device

How much you set up here depends on **why** you're debugging:

- **Exploring or fixing something** — whatever's already installed is fine. If the app is installed and running (e.g. React Native via Metro, or installed manually), skip to step 3.
- **Developing a test case before writing it** (see [Test Cases & Reusable Flows](test-cases.md)) — start from a **clean state** so your walkthrough matches how a saved run executes (every saved run reinstalls the app from the build). Don't rely on leftover state from a prior session. Reset first: [Install iOS](../references/install-ios.md#clean-install-reset-app-state) · [Install Android](../references/install-android.md#clean-install-reset-app-state).

To get the app onto the device:
**Get the build** — ask the user for the path to the binary. If they don't have one, build it first: [iOS Native Builds](../references/builds-ios-native.md) · [React Native Builds](../references/builds-react-native.md) · [Android Builds](../references/builds-android.md).
**Install the build** — [iOS](../references/install-ios.md) · [Android](../references/install-android.md).
**Launch the app** — activate it by bundle ID/App ID. See [Inspect & App Control](../references/inspect-and-app-control.md).

### 3. Inspect the screen and act

Drive the UI as a loop — never fire an action blind:

1. **Inspect** — read the current screen to confirm the target is present and the UI is in the expected state. See [Inspect & App Control](../references/inspect-and-app-control.md).
2. **Act** — perform one action, preferably by describing the target element ([Actions by description](../references/actions-grounding.md)); fall back to explicit relative coordinates (`0–1000`) only if grounding mislocates ([Actions by coordinates](../references/actions-coordinates.md)).
3. **Verify** — inspect the screen again to confirm the state changed as expected, capturing a screenshot for a visual check if needed. See [Inspect & App Control](../references/inspect-and-app-control.md).
4. Repeat for the next action.

App lifecycle, `open-url`, and alert handling are covered in [Inspect & App Control](../references/inspect-and-app-control.md).
