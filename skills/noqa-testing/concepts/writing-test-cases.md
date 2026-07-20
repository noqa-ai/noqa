# Writing Good Test Cases

> This page is the **quality guide** — how to phrase instructions and results so the agent runs them reliably.
> Before saving a case, debug-run the scenario on a device from a clean state — see [Debug on device](../workflows/debug-on-device.md).
> For the data model (what a case, flow, or app context *is*) see [Test Cases](test-cases.md).
> For the step-by-step authoring process and commands see the [workflow](../workflows/test-cases.md).

A good test case reads like instructions you'd hand a careful human tester: clear goals, in order, each with a concrete way to tell it worked. Vague, run-on, or tap-by-tap instructions are the main reason a run fails for the wrong reason.

## Every test starts from a cold launch

This is the single most important thing to internalise. **Each run installs or launches the app fresh — no saved session, no prior navigation, onboarding not yet completed, not logged in.** A test case must describe the whole path from that cold start to the thing you actually want to verify. The app is never already sitting on the screen you care about.

So to test a screen deep inside the app, the run first has to *get there* — through onboarding, login, and any first-launch permission prompts. Three ways to handle that prefix:

- **Reusable flow** (default) — wrap onboarding/login in a reusable flow and reference it by `id` from every case that needs it. Written once, shared everywhere.
- **Deeplink** — if the app has a URL scheme handler (check the codebase), open a deeplink to jump straight to the target screen and skip the navigation.
- **Test build / flag** — a build with onboarding disabled, a feature flag, or pre-seeded data.

Never assume the app is already past onboarding or already logged in.

### Reaching a specific state

Figure out how to reach the state the test needs; if it's not clear from the code, ask the user.

- **Login** — credentials inline (`Sign in with test@example.com / Test1234`) or in app context if shared across many cases. Ask the user for a test account if not clear from the code.
- **Premium / IAP** — ask if there's a sandbox account, a test build with purchases unlocked, or a feature flag. Sandbox purchases work only on local devices.
- **Other state** — ask how to reach it reliably: test build, feature flag, cheat menu, or pre-seeded data.

## Formatting rules

These rules have the biggest effect on quality. Apply them every time.

**1. One flow = one goal.** Split a scenario into flows by goal (sign in / edit profile / verify result). Don't cram a whole scenario into one flow, and don't shatter a single goal into many one-tap flows.

**2. Multi-step instructions = a numbered list, one action per line.** Any flow with more than one action must be written as a numbered list with each action on its own line. Never run actions together in a paragraph — the agent loses track of order and skips steps.

❌ actions run together in prose:
```
Open Settings then toggle Dark mode and go back to Home and check it changed.
```
✅ numbered, one action per line:
```
1. Open the Settings screen.
2. Toggle "Dark mode" on.
3. Return to the Home screen.
```

A single-action flow stays a single line — don't number a list of one:
✅ `Sign in with test@example.com / Test1234.`

**3. Write at the goal level — don't narrate every tap.** Let the agent figure out the taps; describe the intent.
❌ `Tap on button Next on the screen with text "Welcome to App". Tap Next...`
✅ `Complete the onboarding flow until you reach the paywall.`

**4. Get specific only when the agent goes off track.** Start at goal level; add detail only for steps the agent gets wrong.
❌ `Change the name to "John Test".`
✅ `Open the hamburger menu in the top-left. On the Profile screen, tap Edit. Change the display name to "John Test" and tap Save.`

**5. Anchor gestures to specific elements.** A gesture without a target is ambiguous.
- Tap: ❌ `Tap the button.` ✅ `Tap the "Sign in" button at the bottom of the screen.`
- Swipe: ❌ `Swipe left.` ✅ `Swipe left on the "Inbox" card to reveal the delete button.`
- Drag: ❌ `Drag the item up.` ✅ `Drag the "Work" task and drop it above the "Personal" task.`
- Pinch: ❌ `Zoom in.` / `Make it bigger.` ✅ `Pinch to zoom in on the map to reveal street names.` / `Pinch to enlarge the selected "Sale" text sticker.`

## Instructions vs expected result — keep them separate

`instructions` are the **actions to perform**. The expected `result` is **what should be visible on screen afterwards**. Don't hide assertions inside instructions, and don't put actions inside the result.

❌ instructions mixing the check in: `Change the name and make sure it saved.`
✅ instructions (action only): `Change the display name to "John Test" and tap Save.`
✅ result (observation only): `The Profile screen shows "John Test" as the display name.`

### Expected result

Describe what should be visible on screen after the flow. The more concrete, the better. If omitted, the agent only checks that the step completed without errors.

❌ `It works.` ✅ `The confirmation screen shows "Order placed successfully" and an order number.`
❌ `The login fails.` ✅ `The error message "Invalid credentials" is visible on screen.`

## A complete example

> Before saving the flows below, debug-run the whole scenario on a device from a clean state ([Debug on device](../workflows/debug-on-device.md)) to confirm the path and the expected results.

A well-formed case: one goal per flow, a numbered list where a flow has several actions, and a concrete result on each.

```json
{
  "title": "Edit display name from Profile",
  "tags": ["smoke", "profile"],
  "flows": [
    {
      "instructions": "Sign in with test@example.com / Test1234.",
      "result": "The Home screen is visible with the search bar and the recent-items list."
    },
    {
      "instructions": "1. Open the Profile screen from the bottom tab bar.\n2. Tap Edit.\n3. Change the display name to \"John Test\".\n4. Tap Save.",
      "result": "The Profile screen shows \"John Test\" as the display name and a \"Saved\" confirmation."
    }
  ]
}
```

Note: the sign-in step is a separate flow because it's reused across cases — in practice reference it as a reusable flow by `id` instead of inlining it (see the [workflow](../workflows/test-cases.md)).

## Tags

Pass tag names as strings in `cases create` or `cases update` — created automatically if they don't exist.

Conventions: `auth`, `payments`, `smoke`, `regression`, `ios-only`, `android-only`

To see existing tags, use the tags command in [Apps, Cases & Flows](../references/apps-cases-and-flows.md).
