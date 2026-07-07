# Test Cases

> This page is the **data model** — what a case, flow, reusable flow, and app context are.
> For how to phrase good instructions and results see [Writing Good Test Cases](writing-test-cases.md).
> For the authoring process and commands see the [workflow](../workflows/test-cases.md).

noqa is a mobile testing platform. You describe test scenarios in plain English — the AI agent runs them on a real device or simulator and reports pass/fail with screenshots.

## Test case

A test case is a single scenario to verify. It has:
- **Title** — short human-readable name
- **Flows** — one or more steps the agent executes in order
- **Tags** (optional) — for filtering and organizing (e.g. `auth`, `smoke`, `ios-only`)

## Flow

A flow is one step inside a test case:
- **Instructions** — what the agent should do, in natural language
- **Expected result** (optional) — what should be visible on screen after. If omitted, the agent only checks that the step completed without errors.

A flow is either **inline** (unique to this case) or a **reusable flow reference** (shared across cases).

## Reusable flows

A reusable flow is a named flow shared across multiple test cases. When you update it, all test cases that reference it are updated automatically.

Use reusable flows for steps that appear in many cases: login, onboarding, account setup, or any prerequisite state. Reference by `id` when creating or updating a case.

## App context

App context is a shared description injected into every test run automatically. You set it once per app — the agent uses it in every run.

Put here:
- **Credentials** — test account login/password
- **Screen names** — if test cases reference specific screens, define them once here so the agent knows what they are
- **General rules** — things that apply to every test, e.g. `"Always grant any permission prompts that appear"`
- **Core functionality** — brief description of what the app does

Without app context the agent doesn't know what your screens are called or how to log in — you'd have to repeat that in every test case.

**Example app context:**
```
Login: test@example.com / Test1234
Home screen — first screen after login, contains a search bar and a list of recent items.
Profile screen — accessible via the bottom tab bar.
Always grant any permission prompts that appear.
```

**With app context, test instructions can be short:**
```
Sign in and go to the Profile screen.
Search for "shoes" on the Home screen.
```
