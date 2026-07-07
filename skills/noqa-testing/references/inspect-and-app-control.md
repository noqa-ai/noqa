# Inspect & App Control

Shared debug commands: screen inspection, app lifecycle, and alerts. The interactive actions (tap/swipe/drag/input) target elements by plain-language description — see [Actions by description](actions-grounding.md) — or, as a fallback, by explicit relative coordinates (`0–1000`) — see [Actions by coordinates](actions-coordinates.md).

## Screen inspection

```bash
noqa screen                        # compact UI element tree
noqa screen --full                 # full tree with all attributes, can be very long
noqa screenshot /tmp/screen.png    # save screenshot to file, to visually verify
```

**Always inspect before and after any action to verify the actual UI state.**

**Prefer `noqa screen` first — it is the primary way to read the screen.** The element tree is far cheaper in tokens than an image and is enough to understand the UI in most cases. Only fall back to `noqa screenshot` when the tree is not enough: it comes back empty, it lacks the information you need, or you can't tell what's actually rendered (custom drawing, images, visual layout/overlap). Use the screenshot to fill that gap, not as the default way to look at the screen.

Call `noqa screen` directly — do not pipe through `grep`, `awk`, or any other filter. Parse the raw output yourself. Filtering can hide elements you need.

The screen tree includes a `relative_bbox_2d="[xmin, ymin, xmax, ymax]"` attribute (relative `0–1000` on both axes) for each element, giving you its position and size on screen. Use it to understand layout and disambiguate elements when writing a description. It's also what you read for the coordinate fallback: the element center is `x = (xmin + xmax) / 2`, `y = (ymin + ymax) / 2`.

## App lifecycle

```bash
noqa action activate-app --bundle-id com.example.app
noqa action terminate-app --bundle-id com.example.app
noqa action restart-app --bundle-id com.example.app
noqa action background-app
noqa action open-url --url "https://example.com"
```

## Alert

Prefer the `alert` command over tapping a button when interacting with a system alert.

```bash
noqa action alert --action accept
noqa action alert --action dismiss
```
