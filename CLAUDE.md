# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the app

Single file — no build step, no dependencies. Open directly in a browser:

```bash
open index.html
```

Or serve it (required if fetching local resources):

```bash
python3 -m http.server 8080
```

## Architecture

Everything lives in `index.html`: markup, inline `<style>`, and inline `<script>`. No framework, no bundler.

**Step flow** — `step` (integer 0–3) is the single source of truth. `advance()` increments it, hides the current `.screen`, updates the `.step-dash` indicators, shows the next screen, and triggers `showFinal()` on step 3.

**No-button escape** — `#no-btn` is `position:absolute` inside `.s0-btns` (a `position:relative` container). On `mousemove`/`touchmove`, if the cursor is within 130 px of the button, `escapeFrom(mx, my)` computes a 4×3 grid of candidate positions (capped above the YES button zone) and teleports the button to whichever candidate is farthest from the cursor.

**Answer storage** — `ans` object holds `type`, `typeEmoji`, `when`, `whenEmoji`; populated by `pick()` when a grid option is selected.

**Confetti** — pure CSS (`@keyframes cpFall`), spawned dynamically by `confetti()` on step 3.

**Background sparkles** — floated via `setInterval` every 600 ms; each sparkle self-removes after its animation ends.
