# Apps, Cases & Flows

## Apps

```bash
noqa apps list                                                 # list all workspace apps (NAME, PREFIX)
noqa apps get APP                                              # app details, including the current app context
noqa apps get APP --json                                       # raw JSON (use to read the full app_context)
noqa apps update APP '{"name": "...", "app_context": "..."}'   # update app name or context
```

`app_context` is a shared description injected into every test run — use it for credentials, screen names, and general rules the agent should always follow. Read the current value with `noqa apps get APP` before updating, then send the full replacement text (update overwrites, it does not append).

> **Always confirm with the user before running `apps update`** — show the exact JSON you plan to send and wait for explicit approval.

## Cases

A **test case** describes a single scenario to verify — it has a title, optional tags, and a list of flows that define the steps and expected results.

```bash
noqa cases list APP                          # list test cases (table: ID, TITLE, TAGS)
noqa cases list APP --tag smoke              # filter by tag
noqa cases list APP --tag smoke --tag auth   # filter by multiple tags (AND)
noqa cases get APP 42                        # get case details: id, title, tags, flows (JSON)
noqa cases create APP '{"title": "...", "tags": ["smoke"], "flows": [...]}'
noqa cases update APP 42 '{"title": "New title"}'
noqa cases delete APP 42                      # delete a test case (irreversible)
```

> **`cases delete` is destructive and irreversible — always get explicit user approval first.** Name the exact case (number + title) you intend to delete and wait for confirmation before running. Never delete a case the user didn't ask you to remove.

Each case has a `flows` list. A flow must have either `id` OR `instructions`/`result` — never both. Tags are created automatically if they don't exist.

- **inline** — `{"instructions": "...", "result": "..."}` — unique to this case
- **reusable reference** — `{"id": "..."}` — points to a shared reusable flow

> For how to phrase `instructions` and `result` (numbered steps, one goal per flow, concrete results) follow [Writing Good Test Cases](../concepts/writing-test-cases.md).

> **Before writing any inline flow** (on create or update), run `noqa flows APP` and check if a matching reusable flow already exists. If it does — reference it by `id`. If the step will appear in multiple cases — create a reusable flow first, then reference it.

> **Always confirm with the user before running `cases create` or `cases update`** — show the exact JSON you plan to send and wait for explicit approval.

## Reusable flows

A **reusable flow** is a named step shared across multiple test cases — e.g. "Go through onboarding". Instead of repeating the same instructions in every case, reference it by `id`.

```bash
noqa flows list APP                          # list reusable flows (table: ID, NAME, USES)
noqa flows get APP <flow-id>                 # get flow details (JSON: id, name, instructions, result)
noqa flows create APP '{"name": "...", "instructions": "...", "result": "..."}'
noqa flows update APP <flow-id> '{"name": "...", "instructions": "...", "result": "..."}'
noqa flows delete APP <flow-id>              # delete a reusable flow (irreversible)
```

Any combination of `name`, `instructions`, `result` can be updated — omit fields to leave them unchanged.

`flows delete` fails if the flow is still used by any test case — the server returns "Cannot delete flow: it is used in N case(s)". Remove or re-point those cases first.

> **Always confirm with the user before running `flows update`** — show the exact JSON you plan to send and wait for explicit approval.

> **`flows delete` is destructive and irreversible — always get explicit user approval first.** Name the exact flow (id + name) you intend to delete and wait for confirmation before running. Never delete a flow the user didn't ask you to remove.

## Tags

Tags are created automatically when you pass them by name in `cases create` or `cases update`. No separate create step needed.

You may reuse existing tags freely, but confirm with the user before attaching tags to a case — especially when it would create a new tag rather than reuse one from `noqa tags APP`.

```bash
noqa tags APP                      # list tags (table: TAG, USES, CASES)
noqa tags APP --json               # raw JSON output
```
