# Devices

## List devices

```bash
noqa devices ios                   # iOS simulators + physical devices
noqa devices android               # Android emulators + physical devices
noqa devices active                # currently connected device
```

Output columns: TYPE, OS, STATUS, MODEL, ID. Use the full ID to connect.

Physical devices are listed first. Simulators/emulators are capped at 10; use `--all` to see all.

## Connect

```bash
noqa devices connect <device_id>   # open an Appium session on this device
```

**Always check `noqa devices active` before connecting** — only one session at a time.

## Tips

- Prefer a booted simulator over a physical device for speed
- If `noqa devices connect` fails, check that the device is available (`STATUS: available` or `shutdown`)
- iOS physical device requires the noqa app to be trusted on the device
