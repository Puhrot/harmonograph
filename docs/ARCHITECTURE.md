# Architecture Notes

Harmonograph is intentionally a **single-file application** with no build step.

---

## File structure

Everything lives in `index.html`:

```
index.html
├── <head>
│   ├── CDN: p5.js 1.9.4 (cloudflare)
│   ├── CDN: Space Grotesk font (Google Fonts)
│   └── <style> — all CSS
└── <body>
    ├── .sidebar — all controls
    │   ├── #top-panel (paper size, seed, undo/redo, export)
    │   ├── #pens-panel (pen passes)
    │   ├── #global-panel (bg, density, line weight, opacity)
    │   └── #presets-panel (built-in + user presets)
    ├── .canvas-wrap — p5.js canvas mount
    └── <script> — all application logic
```

---

## Key state objects

### `P` — global parameters
```js
let P = {
  seed, steps, dt, strokeW, alpha, colBg
}
```

### `instances[]` — per-pass parameters
```js
{
  color, visible, phaseOffset,
  ratioX, ratioY,            // indices into RATIOS[]
  A1, A2, A3, A4,            // oscillator amplitudes
  p1, p2, p3, p4,            // oscillator phases
  detune, detuneY,           // frequency drift (null = linked)
  decayX, decayY,            // amplitude decay
  offsetX, offsetY,          // canvas translation
  scale, rotation,           // transform
  phases,                    // phase multiplier
  locks: { color, ratioX, ratioY, amp, phaseOffset, detune }
}
```

---

## Rendering pipeline

```
redraw()
  └── render(p)                   ← p5 sketch function
        ├── for each instance
        │     ├── compute totalT = steps × dt
        │     ├── for t = 0 → totalT, step dt
        │     │     ├── x = harmonograph equation (X axis)
        │     │     ├── y = harmonograph equation (Y axis)
        │     │     └── draw line segment
        │     └── repeat × phases (with phase offset increment)
        └── draw paper border
```

Rendering is batched with `requestAnimationFrame` and shows a progress bar for high-density draws.

---

## SVG export

The SVG exporter replays the same math as the canvas renderer but outputs `<line>` elements in real mm coordinates, wrapped in per-pass `<g id="layerN">` elements. Paper size drives the `viewBox` and `width`/`height` attributes.

---

## Undo / redo

State snapshots are stored as JSON strings in `undoStack[]` (max 40). `pushUndo()` serialises `{ P, instances, soloIdx }`. `applySnapshot()` deserialises and rebuilds the UI.

---

## User presets

Saved to `localStorage` with key prefix `hg_preset_`. Export/import uses `JSON.stringify` / `JSON.parse` with a Blob download / FileReader upload.
