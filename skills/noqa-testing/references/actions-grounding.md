# Actions by Description (automatic grounding)

**The primary way to act — prefer this.** Describe the target element in plain language with `--description`/`-d` and noqa resolves its on-screen location for you (vision grounding) — you never compute coordinates. **Free — only requires being signed in** (`noqa status` shows an account). If grounding keeps mislocating a target, fall back to [Actions by coordinates](actions-coordinates.md).

**Always `noqa screen` first** to confirm the target is on screen, then describe it. Base the description **only on what is currently visible** — see [Inspect & App Control](inspect-and-app-control.md).

## Writing a good element description

The quality of grounding depends entirely on the description. Follow these rules:

- **Be specific** — include color, size, icons, and distinctive features (labels, placeholders).
- **Include exact visible text in quotes** — e.g. `"'Continue' button"`, not `"continue button"`.
- **Mention position if relevant** — top/bottom, left/right/center.
- **Describe exactly ONE element** — never use "or", "any", or "similar".
  - ❌ `"'Play as Guest' button or similar option"`
  - ✅ `"green 'Play as Guest' button below the login form"`
- **Make it locatable by a vision model** — clear enough to pick the element out of the screenshot.
- **Describe only what is visible**, not assumptions about the app's UI.

### Tap
Specific element with exact visible text in quotes, color, and position.
```bash
noqa action tap --description "<description>" [--double] [--duration <seconds>]
```
`-d` is a shortcut for `--description`; the examples below use it after the first.
```bash
noqa action tap --description "blue 'Login' button at bottom center"
noqa action tap -d "message item in the conversation list" --duration 3
noqa action tap -d "map area in the center of the screen" --double
```

### Swipe
Describe direction + area + expected outcome.

**Swipe direction is the finger's movement, which is the opposite of the scroll direction.** To scroll **down** the page (reveal content below), swipe **up**; to scroll **up** (reveal content above), swipe **down**. Same for horizontal: to move content left, swipe right, and vice versa. Phrase the description by the finger movement.
```bash
noqa action swipe -d "<description>"
```
```bash
# scroll down to see more items -> finger swipes up
noqa action swipe -d "swipe up on the product list to reveal more items below"
# scroll back to top -> finger swipes down
noqa action swipe -d "swipe down on the product list to return to the top"
```

### Drag
Describe the source element and the target location.
```bash
noqa action drag -d "<description>"
```
```bash
noqa action drag -d "drag list item 'Groceries' above 'Work' to reorder"
```

### Pinch
Two-finger pinch to **zoom** a surface (map/image/camera/page) or **resize** a selected on-canvas object (sticker/text/shape). `in` = zoom in / enlarge, `out` = zoom out / shrink. Describe the area to zoom or the object to resize.
```bash
noqa action pinch --direction <in|out> -d "<description>"
```
```bash
noqa action pinch --direction in -d "map in the center of the screen to reveal street names"
noqa action pinch --direction out -d "photo in the image viewer to fit the whole picture"
noqa action pinch --direction in -d "selected 'Sale' text sticker on the canvas to make it bigger"
```

### Input text
Describe the field by its label or placeholder.
```bash
noqa action input --text <text> -d "<description>"
```
```bash
noqa action input --text "john@example.com" -d "email input field with placeholder 'Email'"
```
