# Harmonograph

A browser-based rotary harmonograph simulator for generative pen plotter art. Configure layered pendulum passes, tune frequencies and phases, then export a ready-to-plot SVG — no build step, no install, no server required.

**[Live demo →](https://puhrot.github.io/harmonograph)**

> 📖 **[User Manual →](Harmonograph_User_Manual.docx)** — Full documentation covering every control, workflow tips, and mathematical reference.

---

## What is a harmonograph?

A harmonograph is a mechanical device that uses pendulums to draw curves. This simulator models a *rotary* harmonograph — two coupled oscillators per axis — producing Lissajous-family curves with optional detuning, amplitude decay, and phase offsets. The results are suitable for direct output to pen plotters (AxiDraw, etc.) via SVG export.

---

## Features

### Multi-pass pen plotting
- Add up to N independent pen passes, each with its own colour, frequency ratios, and transform
- Solo / mute / visibility toggles per pass
- Drag-to-reorder passes
- Duplicate any pass in one click

### Per-pass controls
| Control | Description |
|---|---|
| **Shape** | X/Y frequency ratios (1:1 → 8:1 grid) |
| **Phases** | Multiplier for phase repetitions (1×–8×) |
| **Detune X / Y** | Frequency drift — creates spiral/rosette effects |
| **Asymmetric detune** | Split X and Y drift independently |
| **Decay X / Y** | Amplitude envelope — curve collapses over time |
| **Phase offset** | Rotate the starting phase |
| **X/Y Offset** | Translate the curve on the canvas |
| **Scale** | Scale the curve independently per pass |
| **Rotation** | Rotate the curve in degrees |
| **Amplitudes A1–A4** | Per-oscillator amplitude mix |
| **Phases p1–p4** | Per-oscillator phase values (rad) |

### Global controls
- **Paper size** — A3/A4/A5 portrait & landscape, square formats
- **Background colour**
- **Density** — number of drawn lines (1k–100k)
- **Line weight** — stroke width in mm
- **Opacity** — stroke alpha (3–255)
- **Seed** — random seed for reproducible results

### Randomisation & presets
- **RND** per pass — randomise a single pass
- **Randomise All** — regenerate all passes at once
- **Parameter locking** — lock any parameter from randomisation (Col, RX, RY, Amp, Ph, Det)
- **Built-in presets** — Lobed, Trefoil, Star, Spiral, Ribbon, Lattice, Rose, Figure-8
- **User presets** — save/load to browser localStorage, export/import as JSON

### Undo / Redo
- Full undo/redo history (40 states)
- Keyboard shortcuts: `Ctrl+Z` / `Ctrl+Y` (or `Ctrl+Shift+Z`)

### Export
- **SVG export** — per-layer `<g>` elements in real mm coordinates, print-ready
- All values editable inline — click any displayed number to type a value directly

---

## Usage

Open `index.html` in any modern browser. No installation required.

```bash
# Clone and open
git clone https://github.com/nate-northrup/harmonograph.git
cd harmonograph
open index.html   # macOS
# or just drag index.html into your browser
```

### Quick start

1. Choose a **paper size** from the top panel
2. Pick a **preset** from the Presets section, or hit **RND** on a pass to generate something random
3. Adjust **Shape** (frequency ratios) and **Detune** to taste
4. Add more pen passes with **+ Add Pen Pass** for layered colour work
5. Hit **Export SVG** when ready

---

## File structure

```
harmonograph/
├── index.html                    # The entire application (single file)
├── Harmonograph_User_Manual.docx # Full user manual
├── docs/
│   ├── CONTROLS.md               # Full parameter reference
│   └── ARCHITECTURE.md           # Code structure notes
├── CHANGELOG.md
├── LICENSE
└── package.json                  # Project metadata only (no build step)
```

---

## Dependencies

Loaded via CDN — no local install needed:

| Library | Version | Purpose |
|---|---|---|
| [p5.js](https://p5js.org) | 1.9.4 | Canvas rendering |
| [Space Grotesk](https://fonts.google.com/specimen/Space+Grotesk) | — | UI font |

---

## License

MIT — see [LICENSE](LICENSE)
