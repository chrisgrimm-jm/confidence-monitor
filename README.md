# Confidence Monitor

An in-studio talent confidence monitor. Shows the OBS **program feed** with local-only overlays — clock, timer, producer notes, big talent text, an embedded teleprompter, and live YouTube chat. The overlays render **only on this monitor** and never touch the OBS output, so the broadcast/vdo.ninja feed is unaffected.

Single self-contained HTML file. No build, no server, no dependencies.

## Setup (same machine as OBS)
1. Open the [page](https://chrisgrimm-jm.github.io/confidence-monitor/) — that's the **Control** panel.
2. Click **Open Talent Display** — a second window opens.
3. In OBS: right-click the program preview → **Windowed Projector (Program)**.
4. In the Display window: click **Start Feed** and pick that projector window. Drag the Display to the talent's monitor and press **Fullscreen**.
5. Drive everything from Control — it updates the Display live.

The OBS Windowed Projector can sit anywhere (even hidden); `getDisplayMedia` captures its pixels directly. You put the **Display window** on the talent monitor, not the projector.

## Overlays
- **Talent text** — big read-this text; position Top / Middle / Bottom.
- **Producer note** — a private aside; 9-point placement.
- **Timer** — countdown or count-up; 9-point placement; turns red under 10s.
- **Clock** — wall-clock, top-right.
- **Teleprompter** — embeds the [prompter](https://chrisgrimm-jm.github.io/prompter/) display (Firebase-synced). Drive scripts/play/speed from the prompter's own control page; match the `?topic=` in both URLs. Modes: Full / Top band / Bottom band.
- **YouTube chat** — embeds YouTube's live chat as a side panel. Only renders for a **live** video, and only when this page is hosted (not `file://`).

## How it syncs
Control and Display run on the same machine. Control opens the Display and sends state over `postMessage`; the Display is a pure renderer. Control also persists to `localStorage`, so a refresh keeps your setup. The teleprompter and YouTube chat are embedded iframes that sync themselves.

---
Jomboy Media · hosted on GitHub Pages
