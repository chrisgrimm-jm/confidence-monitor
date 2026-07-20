# Confidence Monitor

An in-studio talent confidence monitor. Shows the OBS **program feed** with local-only overlays — producer notes, a timer, a clock, an embedded teleprompter, an embedded web page, and live YouTube chat. The overlays render **only on this monitor** and never touch the OBS output, so the broadcast/vdo.ninja feed is unaffected.

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
- **Web page** — embed any URL (a notes page, a published Google Doc, a `/preview` link). Paste a URL and hit **Load**; a live, interactive copy appears in the Control panel so the producer can scroll/click/type, and the same URL shows on the talent monitor. Modes: Full / Top band / Bottom band.
  - Pages that block embedding won't load. The Google Docs **editor** is one of them — use **File → Share → Publish to web** or a `/preview` link (read-only, auto-updates as you edit the doc in a normal tab).
  - Cross-origin pages can't be scroll-synced between windows, so the producer interacts with their own copy; collaborative content (a Google Doc) syncs through the page's own backend.
- **YouTube chat** — embeds YouTube's live chat as a Left/Right side panel. Only renders for a **currently live** video, and only when this page is hosted (not `file://`).

## How it syncs
Control and Display run on the same machine. Control opens the Display and sends the whole state over `postMessage` on every change; the Display is a pure renderer. Control also persists to `localStorage`, so a refresh keeps your setup. The teleprompter, web page, and YouTube chat are embedded iframes that sync themselves.

---
Jomboy Media · hosted on GitHub Pages
