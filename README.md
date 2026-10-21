# The Same Day, Again

A ~18-second looping stick-figure short about someone who does not love their job — the alarm, the sad freshen-up, the suit and tie, the drag to work, the slouch at the desk, the traffic home, the change into home clothes, and sleep… which flows straight back into the alarm.

Everything is generated **purely in JavaScript** — Canvas 2D for drawing, `MediaRecorder` + `canvas.captureStream()` for export. No libraries, no build step, no server.

## Run it

Open `index.html` in a modern browser (Chrome, Edge, or Firefox). Double-click the file, or:

```bash
# optional: serve it (any static server works)
npx serve .
```

The animation plays and loops automatically.

## Controls

- **Pause / Play** — or press `Space`
- **Restart loop** — jump back to the alarm
- **Export video (.webm)** — records exactly one loop and downloads `the-same-day-again.webm`

## Why it loops seamlessly

The entire frame is a pure function of a single time value `t` in `[0, DUR)`. Every scene fades from and to black at its boundaries, so the wrap-around (last frame → first frame) is black→black — there is no visible seam. The final pose (asleep in bed) also matches the opening pose.

## Convert to MP4 (optional)

The export is `.webm`. If you need `.mp4`:

```bash
ffmpeg -i the-same-day-again.webm -movflags +faststart -pix_fmt yuv420p the-same-day-again.mp4
```

## Structure

Single file, `index.html`:
- **Math + palette** helpers
- **`drawFigure` / `limb` / `drawFace`** — the articulated stick figure (poses are joint angles; `0` = pointing down)
- **Backgrounds** — bedroom, bathroom, wardrobe, street, office, evening road
- **Scenes** — `sWake`, `sFreshen`, `sSuit`, `sDrag`, `sWork`, `sTraffic`, `sHome`
- **Timeline** — `SCENES`, `renderFrame(t)`, fade-to-black boundaries
- **Playback + MediaRecorder export**
