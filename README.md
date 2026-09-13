# bloop-abc-123

A keyboard-driven bubble game for toddlers to learn letters and numbers. Press a key, a bubble pops up. Press backspace, the last bubble pops away. That's it.

Built as a single HTML file — no framework, no build step, no dependencies.

---

## How it works

| Key | What happens |
|---|---|
| Any **letter** (A–Z) | A colourful bubble appears with that letter inside |
| Any **number** (0–9) | A colourful bubble appears with that number inside |
| **Backspace** | The most recently added bubble pops away |
| Anything else | Error sound + brief red screen flash |

Bubbles appear **left to right, top to bottom** — just like typing in a text editor. When a row is full the next bubble wraps to the next line. When the whole screen is full, pressing any key plays the error sound until backspace makes room.

Once **51 bubbles** have been created, the game ends with a "🎉 Great job baby!" message spoken aloud and displayed on screen. All further keypresses are ignored until the page is refreshed.

Every bubble speaks its letter or number aloud when it appears (Web Speech API). Popping sounds use the Web Audio API — no audio files needed.

---

## Features

- **51-bubble win condition** — after 51 bubbles have been created a "🎉 Great job baby!" message appears with a golden glow animation, spoken aloud via speech synthesis; all further keypresses are disabled until the page is refreshed
- **No menu, no loading screen** — opens straight to the game
- **Glossy animated bubbles** — each one bobs gently at its own speed and phase
- **Springy spawn animation** — bubbles bounce in with a slight overshoot
- **Pop animation** — white flash then implodes with an expanding ring
- **Three audio layers** per event, synthesised in real time via Web Audio API
  - Spawn: rising chirp
  - Pop: higher-pitched burst
  - Error: descending buzz
- **Speech** — Web Speech API reads each character aloud on key release
- **Responsive** — reflowing on orientation change; works in portrait and landscape
- **PWA-ready** — `manifest.json` + apple-touch-icon support for home screen install

---

## Files

```
index.html          # The whole game
manifest.json       # PWA manifest
pwa.js              # Install prompt handler (beforeinstallprompt)
icon-192.png        # App icon 192×192
icon-512.png        # App icon 512×512
```

The game lives entirely in `index.html`. `pwa.js` is the only external script and only handles the install banner — the game works without it.

---

## Running locally

No build step needed. Just serve the files over HTTP:

```bash
# Python
python3 -m http.server 8080

# Node
npx serve .
```

Then open `http://localhost:8080` in any modern browser and start typing.

> Opening `index.html` directly as a `file://` URL will work for the game itself but the PWA install prompt and some Speech Synthesis voices won't be available.

---

## Deploying

Any static host works — GitHub Pages, Netlify, Vercel, Cloudflare Pages.

**GitHub Pages (quickest):**

1. Push the repo to GitHub
2. Go to **Settings → Pages → Source** and pick your branch
3. The game is live at `https://sunandandhawan.github.io/bloop-abc-123/`

For the PWA to be installable the site must be served over HTTPS, which all of the above hosts do by default.

---

## Customising

**Bubble size** — change `BUBBLE_SIZE` at the top of the script (default `110` px). The grid recomputes automatically.

**Bubble colours** — edit the `COLORS` array. Any valid CSS hex colour works.

**Gap between bubbles** — change `GAP` (default `14` px).

**Margin from screen edges** — change `MARGIN` (default `16` px).

**Speech pitch / rate** — adjust `u.pitch` and `u.rate` inside the `speak()` function.

---

## Browser support

Requires a modern browser with support for:

- [Web Audio API](https://caniuse.com/audio-api) — all modern browsers
- [Web Speech API](https://caniuse.com/speech-synthesis) — Chrome, Edge, Safari (Firefox partial)
- [CSS `dvh`/`dvw` units](https://caniuse.com/viewport-unit-variants) — Chrome 108+, Safari 15.4+, Firefox 116+
- [ResizeObserver](https://caniuse.com/resizeobserver) — all modern browsers

Works best on Chrome (desktop and Android) and Safari (iOS). Firefox plays audio and shows bubbles but speech synthesis voices may be limited.

---

## Designed for

Toddlers aged 1–4 sitting on a parent's lap at a keyboard. The parent opens the page, hands over the keyboard, and the child discovers that every key press makes something happen. The game teaches key-to-character mapping, letter and number recognition, and cause-and-effect — without scores, timers, or failure states beyond the gentle error buzz.
