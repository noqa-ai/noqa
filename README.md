<div align="center">

<img src="https://cdn.noqa.ai/media/logo.png" alt="noqa" width="240" />

### Agentic QA automation for mobile apps and games

**[noqa.ai](https://noqa.ai)** · **[Documentation](https://docs.noqa.ai)** · **[Download (Apple Silicon)](https://cdn.noqa.ai/app/macos/releases/latest/noqa.dmg)**

</div>

noqa is an AI agent that tests mobile apps and games from natural-language test cases. It works purely from screenshots — reading the screen like a human tester, with no accessibility tree or view hierarchy — and controls the real app on iOS and Android devices, simulators, or in the cloud, returning per-step pass/fail with traces and screenshots.

- **Any framework, even games — no locators** — native, React Native, Flutter, Unity, Unreal, and non-native UI (maps, canvases, WebViews, ads) all test the same, with no locators or scripts.
- **CLI with automatic grounding** — your coding agent drives a real device from the terminal: read the screen, act by plain-language description (noqa grounds it — no coordinates), and manage test cases.

  `noqa action tap -d "Blue login button at the bottom"`
- **Runs on real devices** — plus simulators, emulators, and cloud devices.

<div align="center">

<video src="https://raw.githubusercontent.com/noqa-ai/noqa/main/assets/demo.mp4" controls muted loop playsinline width="820"></video>

</div>

## Two ways to use noqa

<table width="100%">
<tr><th align="left" width="50%">Free</th><th align="left" width="50%">With a subscription</th></tr>
<tr>
<td valign="top">
• CLI with automatic grounding<br>
• Test case management<br>
• Your local real devices, simulators, and emulators
</td>
<td valign="top">• The noqa agent, purpose-built for visual mobile testing, runs your suite locally or in the cloud<br>• Cloud devices<br>• Reports with video and screenshots<br>• Integrations – Slack, Jira, Webhooks, API, MCP server</td>
</tr>
</table>

## Quick start

**1. Install the app.** [Download noqa](https://cdn.noqa.ai/app/macos/releases/latest/noqa.dmg) and keep it running — the CLI talks to it locally.

**2. Install the CLI and skill.** In the app, open **Settings → Tools → Install CLI** (adds `noqa` to your `PATH`) — and click **Install skill** too, so your coding agent knows the commands out of the box. Restart your terminal afterward. The skill also lives here in [`skills/noqa-testing`](skills/noqa-testing) if you'd rather add it manually.

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
noqa action tap -d "Login button"              # act: one step, by description — grounding (recommended)
noqa action tap --x 500 --y 320                # …or by relative coordinates (0–1000) as a fallback
noqa screen                                    # verify: confirm the state changed as expected
```

Full command reference: [docs.noqa.ai/docs/cli](https://docs.noqa.ai/docs/cli).
