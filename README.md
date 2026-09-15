# Confidence Monitor

An in-studio talent confidence monitor. Shows the OBS **program feed** with local-only overlays — producer notes, a timer, a clock, a full built-in teleprompter, and live YouTube chat. The overlays render **only on this monitor** and never touch the OBS output, so the broadcast/vdo.ninja feed is unaffected.

Single self-contained HTML file. No build, no server. One dependency: Firebase (Realtime Database), used only for the teleprompter's script library — everything else is `postMessage`/`localStorage` between the two windows on this one machine.

Absorbs the standalone [Prompter](https://github.com/chrisgrimm-jm/prompter) app — its script library, editor, transport, and appearance controls now live directly in this Control panel, and its scroll-and-display logic is native on this Display (no more separate prompter Control/Display windows to babysit alongside OBS and this app).

## Setup (same machine as OBS)
1. Open the [page](https://chrisgrimm-jm.github.io/confidence-monitor/) — that's the **Control** panel.
2. Click **Open Talent Display** — a second window opens.
3. In OBS: right-click the program preview → **Windowed Projector (Program)**.
4. In the Display window: click **Capture Feed** and pick that projector window (pick **Window**, not **Screen** — see note below). Drag the Display to the talent's monitor and press **Fullscreen**.
5. Drive everything from Control — it updates the Display live.

The OBS Windowed Projector can sit anywhere (even hidden); `getDisplayMedia` captures its pixels directly. You put the **Display window** on the talent monitor, not the projector — even if the Display window ends up completely covering the projector on the same monitor, window capture keeps working since it grabs that window's own render buffer, not whatever's on top of it. Capturing **Entire Screen** instead of a window will feed back into itself if the Display is fullscreen on that same monitor, so always pick the specific window.

If the capture picker only shows one window to choose from, that's almost always a macOS permission issue, not the app: grant the browser **Screen Recording** access in System Settings → Privacy & Security, then fully quit and relaunch the browser (not just reload the page).

Each overlay has a **SHOW/HIDE** button and, where relevant, a position/mode dropdown — all in the card header on the Control panel. Producer Note, Timer, and Clock also have a **Size** field for font size in px.

## Overlays
- **Producer note** — a text aside for the talent; 9-point placement (corners / edges / center); adjustable size.
- **Timer** — countdown or count-up; 9-point placement; turns red under 10s; adjustable size.
- **Clock** — wall-clock time of day, top-right; adjustable size.
- **Teleprompter** — built in. Script library (paste ad copy straight from a Google Doc — colors/highlights carry over, or link a published Doc for auto-refresh), rich-text editor with trim points, transport (play/pause/scrub/nudge), and appearance (font size, line spacing, margins, theme, mirror/flip, reading line) — all in the Control panel's Teleprompter card. Synced to the Display via Firebase under a **Topic** (default `adread`; change it to run a different session — building/editing the library ahead of time from any device still works, same as the standalone app did). Modes: Full / Top band / Bottom band. Can be triggered externally — see **Companion / hardware triggers** below.
- **YouTube chat** — embeds YouTube's live chat as a Left/Right side panel. Only renders for a **currently live** video, and only when this page is hosted (not `file://`).

## Companion / hardware triggers
A read can be put live from outside the browser — a Bitfocus Companion button, a Stream Deck, anything that can fire an HTTP request — by writing directly to the same Firebase Realtime Database the app already uses (open/unauthenticated, same as every other read/write this app does; no server of its own to run).

In Companion, add a **Generic → HTTP Request** action per button:
- Method: `PATCH`
- URL: `https://pinpoint-abf21-default-rtdb.firebaseio.com/prompter/<topic>/trigger.json` (use the Topic shown in the Teleprompter card, `adread` by default)
- Header: `Content-Type: application/json`
- Body: `{"name":"<exact read name>","n":{".sv":"timestamp"}}`

The name is matched case-insensitively against the Script Library. `n` must be Firebase's server-timestamp placeholder (`{".sv":"timestamp"}`), not a fixed number — otherwise pressing the same button twice in a row won't fire the second time, since the app only reacts when the value increases. A name that doesn't match anything currently in the library shows an error in the Edit-read message area rather than silently doing nothing.

## How it syncs
Control and Display run on the same machine. Control opens the Display and sends the whole state over `postMessage` on every change; the Display is a pure renderer. Control also persists to `localStorage`, so a refresh keeps your setup. The program feed is a screen capture (`getDisplayMedia`) set up once in the Display; YouTube chat is an embedded iframe that syncs itself.

The teleprompter is the exception: script library, active content, playback settings, and transport commands all flow over **Firebase** (shared `pinpoint-abf21` project, under `prompter/{topic}`), the same as the standalone app did — Control writes, Display reads, independent of `postMessage`. Only the Topic string and the SHOW/HIDE + Full/Top/Bottom mode travel over the regular `postMessage`/`localStorage` state, since those are this app's own layout concerns.

---
Jomboy Media · hosted on GitHub Pages
