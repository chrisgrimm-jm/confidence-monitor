# Confidence Monitor

An in-studio talent confidence monitor. Shows the OBS **program feed** with local-only overlays — producer notes, a timer, a clock, an embedded teleprompter, a mirrored web page / doc, and live YouTube chat. The overlays render **only on this monitor** and never touch the OBS output, so the broadcast/vdo.ninja feed is unaffected.

Single self-contained HTML file. No build, no server, no dependencies.

## Setup (same machine as OBS)
1. Open the [page](https://chrisgrimm-jm.github.io/confidence-monitor/) — that's the **Control** panel.
2. Click **Open Talent Display** — a second window opens.
3. In OBS: right-click the program preview → **Windowed Projector (Program)**.
4. In the Display window: click **Start Feed** and pick that projector window. Drag the Display to the talent's monitor and press **Fullscreen**.
5. Drive everything from Control — it updates the Display live.

The OBS Windowed Projector can sit anywhere (even hidden); `getDisplayMedia` captures its pixels directly. You put the **Display window** on the talent monitor, not the projector.

Each overlay has a **SHOW/HIDE** button and, where relevant, a position/mode dropdown — all in the card header on the Control panel.

## Overlays
- **Producer note** — a text aside for the talent; 9-point placement (corners / edges / center).
- **Timer** — countdown or count-up; 9-point placement; turns red under 10s.
- **Clock** — wall-clock time of day, top-right.
- **Teleprompter** — embeds the [prompter](https://chrisgrimm-jm.github.io/prompter/) display (Firebase-synced). Drive scripts/play/speed from the prompter's own control page; match the `?topic=` in both URLs. Modes: Full / Top band / Bottom band.
- **Web / Doc** — mirrors any browser window onto the talent monitor via screen capture (same mechanism as the program feed). In the Display window click **Capture Window** and pick the browser window showing your page. The producer then works in that real window on their own screen — scrolling, clicking, editing a live Google Doc — and the talent sees the pixels in real time. Works with **any** site including the Google Docs editor (no iframe/embedding restrictions). Read-only mirror for the talent. Modes: Full / Top band / Bottom band.
- **YouTube chat** — embeds YouTube's live chat as a Left/Right side panel. Only renders for a **currently live** video, and only when this page is hosted (not `file://`).

## How it syncs
Control and Display run on the same machine. Control opens the Display and sends the whole state over `postMessage` on every change; the Display is a pure renderer. Control also persists to `localStorage`, so a refresh keeps your setup. The program feed and the Web/Doc layer are screen captures (`getDisplayMedia`) set up once in the Display; the teleprompter and YouTube chat are embedded iframes that sync themselves.

---
Jomboy Media · hosted on GitHub Pages
