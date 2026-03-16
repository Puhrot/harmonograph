# CLAUDE.md — Harmonograph

## Project Overview

Browser-based rotary harmonograph simulator for generative pen plotter art. Models two coupled oscillators per axis to produce Lissajous-family curves with layered pen passes, tunable frequencies/phases, and SVG export for pen plotters (AxiDraw, etc.).

- **Live demo:** https://puhrot.github.io/harmonograph
- **License:** MIT
- **Author:** Nate Northrup

## Architecture

**Single-file application.** Everything lives in `index.html` (~1500 lines) — HTML structure, CSS styles, and all JavaScript. There is no build step, no bundler, no framework. The app loads p5.js v1.9.4 from CDN and runs directly in the browser.

### Key state objects

- `P` — Global parameters (seed, steps, dt, strokeW, alpha, colBg)
- `instances[]` — Array of pen pass objects (colour, oscillator params, transforms, locks)
- `RATIOS[]` — 8 predefined frequency ratio pairs
- `PAPERS` — 10 standard paper size definitions
- `PRESETS` — 14 built-in named preset configurations

### Rendering pipeline

```
redraw() → render(p5)
  ├── Set canvas background
  ├── For each visible instance:
  │   ├── computePoints(inst, rotAngle) → screen + paper coordinates
  │   └── Store points and colour
  ├── Batch render lines with requestAnimationFrame + progress bar
  └── Draw paper overlay (margin guide)
```

### SVG export

`exportSVG()` converts screen coordinates to millimetres, generates `<line>` elements grouped per layer in `<g id="layerN">`, and outputs with viewBox and margin guide for direct pen plotter use.

### Undo/redo

40-state stack (`undoHistory`, `undoPos`). Snapshots are JSON: `{ P, instances, soloIdx }`. Keyboard: `Ctrl+Z` undo, `Ctrl+Y` / `Ctrl+Shift+Z` redo.

### User presets

Stored in `localStorage` with key prefix `hg_preset_`. Supports JSON import/export via file download/upload.

## File Structure

```
index.html                     # Entire application (HTML + CSS + JS)
package.json                   # Metadata only — no dependencies
README.md                      # Feature overview and quick start
CHANGELOG.md                   # Release notes
LICENSE                        # MIT
Harmonograph_User_Manual.pdf   # Complete user guide
docs/
  ARCHITECTURE.md              # Code structure and state objects
  CONTROLS.md                  # Parameter reference and keyboard shortcuts
```

## Development Setup

No install or build required. Open `index.html` in any modern browser, or:

```sh
npm start   # macOS only — runs `open index.html`
```

There are no npm dependencies, no node_modules, no build tools. The only external dependency is p5.js loaded from CDN at runtime.

## Code Conventions

### Keep everything in index.html

Do not split into separate files. The single-file design is intentional for simplicity and portability.

### Naming patterns

- **Global params:** `P` object with short keys (`seed`, `steps`, `dt`, `strokeW`, `alpha`, `colBg`)
- **Handlers:** `on*` prefix (`onPaperChange`, `onBgChange`, `onCardDragStart`)
- **Mutators:** `set*` prefix (`setInstColor`, `setInstPhase`, `setInstScale`)
- **Toggles:** `toggle*` prefix (`toggleInstVisible`, `toggleAsymDetune`)
- **Randomizers:** `randomize*` prefix (`randomizeInstance`, `randomizeAll`)
- **UI helpers:** Short names (`sv`, `csW`, `csH`, `ampUnit`)
- **DOM IDs:** `v-*` for value displays, `i*` for inputs, `btn-*` for buttons, `sw-*` for colour swatches

### Code sections

Mark sections with `// ─── SECTION NAME ──────────` comment headers using unicode box-drawing characters.

### State mutation pattern

All parameter changes must:
1. Call `pushUndo()` before mutating (for major user actions)
2. Update the state (`P`, `instances[]`)
3. Sync all related UI elements (slider + input + display value)
4. Call `redraw()` to trigger re-render

### Colours

Use 6-digit lowercase hex format: `#3355bb`.

### Paper coordinates

SVG export always uses millimetres for pen plotter compatibility. Screen coordinates are converted via `computeScale()`.

### Pass numbering

UI shows 1-indexed passes ("Pass 1", "Pass 2") while code uses 0-indexed arrays.

### Lock semantics

`true` = locked (excluded from randomization), `false` = unlocked.

## Testing

No automated tests are configured. All testing is manual via the browser. Key areas to verify when making changes:

- Rendering pipeline (`computePoints`, render batching)
- Undo/redo history (40-state stack)
- SVG export accuracy (mm coordinate conversion)
- Preset save/load (localStorage serialization)
- Drag-to-reorder pen passes
- Responsive canvas sizing

## CI/CD

None configured. Deployed as static files to GitHub Pages.

## Key Functions Reference

| Function | Purpose |
|----------|---------|
| `computePoints(inst, rotAngle)` | Generate Lissajous coordinates |
| `render(p5)` | Main render loop with batching |
| `buildSketch(p5)` | p5.js setup configuration |
| `pushUndo()` / `undo()` / `redo()` | History management |
| `applySnapshot(state)` | Restore from JSON |
| `buildInstancesUI()` | Generate all pen pass cards |
| `exportSVG()` | Generate and download SVG |
| `randomizeAll()` / `randomizeInstance(idx)` | Randomize parameters |
| `applyPreset(name)` | Load built-in preset |
| `saveUserPreset()` / `loadUserPreset(name)` | User preset management |
| `redraw()` | Trigger re-render |
