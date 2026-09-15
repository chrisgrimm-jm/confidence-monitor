# Confidence Monitor

An in-studio talent confidence monitor. Shows the OBS **program feed** with local-only overlays — producer notes, a timer, a clock, an embedded teleprompter, and live YouTube chat. The overlays render **only on this monitor** and never touch the OBS output, so the broadcast/vdo.ninja feed is unaffected.

Single self-contained HTML file. No build, no server, no dependencies.

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
- **Teleprompter** — embeds the [prompter](https://chrisgrimm-jm.github.io/prompter/) display (Firebase-synced). Drive scripts/play/speed from the prompter's own control page; match the `?topic=` in both URLs. Modes: Full / Top band / Bottom band.
- **YouTube chat** — embeds YouTube's live chat as a Left/Right side panel. Only renders for a **currently live** video, and only when this page is hosted (not `file://`).

## How it syncs
Control and Display run on the same machine. Control opens the Display and sends the whole state over `postMessage` on every change; the Display is a pure renderer. Control also persists to `localStorage`, so a refresh keeps your setup. The program feed is a screen capture (`getDisplayMedia`) set up once in the Display; the teleprompter and YouTube chat are embedded iframes that sync themselves.

---
Jomboy Media · hosted on GitHub Pages
