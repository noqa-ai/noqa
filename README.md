<div align="center">

<img src="https://cdn.noqa.ai/media/logo.png" alt="noqa" width="240" />

### Agentic QA automation for mobile apps and games

`iOS` · `Android` · `Web`

</div>

noqa is an AI agent that tests mobile apps and games from natural-language test cases — no locators or scripts to maintain. It drives the real app on iOS and Android devices, simulators, or cloud, and returns per-step pass/fail with traces and screenshots.

<div align="center">

<video src="https://cdn.noqa.ai/media/demo.mp4" controls muted loop playsinline width="820"></video>

</div>

**noqa works purely from screenshots** — it reads the screen like a human tester, with no accessibility tree or view hierarchy. That means:

- **Works with any framework, even games** — native iOS/Android, React Native, Flutter, Unity, Unreal, and other frameworks all work the same.
- **No locators or scripts** — write steps in plain language, e.g. `"tap the Start button at the bottom of the screen"`.
- **Tests non-native UI** — maps, canvases, WebViews, in-app ads, and other custom-rendered content that breaks tree-based automation.

**Let your agent test on real devices** — point any coding agent (Claude Code, Cursor, Codex) at the CLI to read the screen and drive the app.

- **CLI to debug your app on device** — inspect the screen, tap, type, swipe, and verify the result, step by step from the terminal.
- **Works on real devices** — plus simulators, emulators, and cloud devices.

### Automate the full QA cycle with an account

With an account, the noqa agent runs your whole suite — write test cases once and hand them off to run autonomously. Paid plans unlock:

- **Purpose-built agent** — tuned for visual testing: ~5s per action, ~1s with memory, at $0.02/action — cheaper than driving an LLM API yourself.
- **1000+ cloud devices** — run the suite in parallel, no hardware to manage.
- **Test management** — organize cases, tags, and reusable flows; get reports with video and screenshots.
- **Team integrations** — Slack, Jira, CI/CD, Webhooks, API, and an MCP server.

## Quick start

**1. Install the app.** [Download noqa](https://cdn.noqa.ai/app/macos/releases/latest/noqa.dmg) and keep it running — the CLI talks to it locally.

**2. Install the CLI and skill.** In the app, open **Settings → Tools → Install CLI** (adds `noqa` to your `PATH`) — and click **Install skill** too, so your coding agent knows the commands out of the box. Restart your terminal afterward.

**3. Connect a device.** List devices, pick one, and connect:

```bash
noqa devices ios                       # list iOS devices & simulators (or: noqa devices android)
noqa devices connect <device-id>       # connect to one from the list
```

**4. Get your app on the device.** Install your build (Xcode, Expo, Android Studio) and bring it to the foreground:

```bash
noqa action activate-app --bundle-id com.example.app
```

**5. Debug it in an inspect → act → verify loop.**

```bash
noqa screen                                    # inspect: read the current UI as a structured element tree
noqa action tap --description "Login button"   # act: one step, by description — grounding (recommended)
noqa action tap --x 500 --y 320                # …or by relative coordinates (0–1000) as a fallback
noqa screen                                    # verify: confirm the state changed as expected
```

Full command reference: [docs.noqa.ai/docs/cli](https://docs.noqa.ai/docs/cli).
