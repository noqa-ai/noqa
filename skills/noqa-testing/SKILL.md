---
name: noqa-testing
description: Use this skill when the user wants to boot and interact with iOS or Android devices/simulators — inspect the screen, execute actions, run UI tests, or manage test cases via the noqa platform.
---

# noqa Device Testing

noqa controls iOS/Android devices and simulators through the `noqa` CLI. There are two modes of interaction: you drive the device yourself (debug), or the noqa agent runs saved test cases for you.

Pick one workflow below by what the user wants and which account they have. Follow that workflow's file — it links to the exact commands in `references/`.

## Before you start

Every workflow needs the user to be **signed in** (`noqa status` shows an account). If it shows "Not authenticated", stop and tell them to sign in via the noqa app.

1. **Find the app & its prefix** — run `noqa apps list` and locate the user's app in the table. Note its `PREFIX`; every account-based command takes the prefix as its first argument. See [Apps, Cases & Flows](references/apps-cases-and-flows.md).
2. **Determine the account type** — run `noqa status` and read off which workflows are available:

   | `noqa status` shows | Account type | Available workflows |
   |---|---|---|
   | signed in, no subscription, credits = $0.00 | **Free** | 1, 2 |
   | active subscription **or** credits > $0.00 | **Paid** | 1, 2, 3, 4 |

   If a workflow needs a paid account and the user is on a free account, stop and tell them to add credits or subscribe at https://noqa.ai.

## Workflows

### 1. Debug on device — any signed-in account

You drive the device: inspect the screen, then tap/swipe/drag/type via `noqa action`. **Prefer describing the target element in plain language with `--description`** and noqa resolves its location for you (grounding — free, just needs sign-in); see [Actions by description](references/actions-grounding.md). As a fallback, you can pass explicit relative coordinates — [Actions by coordinates](references/actions-coordinates.md). Requires a connected device; install a build and launch by bundle ID only if the app isn't already running. Use when the user wants to inspect or manually exercise an app.

- Workflow: [Debug directly on device](workflows/debug-on-device.md)

### 2. Test Management — free account

Create and edit test cases, reusable flows, tags, and the app context. Does not run anything on a device. Use when the user wants to author or maintain the test suite.

- Workflow: [Create & update test cases](workflows/test-cases.md)
- Concepts: [Test Cases, Flows & App Context](concepts/test-cases.md) · [Writing Good Test Cases](concepts/writing-test-cases.md)

### 3. Run saved test cases — paid account

Run already-saved cases by number on the connected device — for smoke/regression. Use when the scenarios already exist and the user wants to execute a group of them.

- Workflow: [Run saved tests with the noqa agent](workflows/run-tests.md)

### 4. Run tests in cloud

Not implemented yet — in development.

## Rules

- If any command fails with "Not authenticated", tell the user to sign in via the noqa app
- If any command reports that noqa is not running / not reachable / "start it first", the noqa desktop app simply isn't open. Launch it yourself — `open -a noqa` — then retry the command. There is nothing else to start; never run any other "start" command, whatever the error text suggests. If the app doesn't come up, ask the user to open noqa.app.
- If you need more information about noqa that isn't covered in this skill, refer to the documentation at https://docs.noqa.ai/llms.txt
