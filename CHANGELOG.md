# Changelog

All notable changes to this project will be documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

---

## [1.0.0] — 2026-03-13

Initial release.

### Added

**Multi-pass system**
- Multiple independent pen passes, each with colour, shape, and transform settings
- Visibility toggle (show/hide) per pass
- Solo mode — isolate a single pass for preview
- Duplicate pass in one click
- Drag-to-reorder passes

**Shape controls**
- X/Y frequency ratio grids (1:1 → 8:1)
- Phase multiplier (1×, 2×, 3×, 4×, 6×, 8×)
- Detune X — frequency drift for spiral/rosette effects
- Asymmetric detune — independent X/Y drift
- Amplitude decay X/Y — curve envelope over time
- Per-oscillator amplitudes A1–A4 and phases p1–p4

**Transform controls**
- Phase offset
- X/Y canvas offset
- Scale
- Rotation (degrees)
- Reset transform button

**Global controls**
- Paper size selector (A3/A4/A5 portrait & landscape, square)
- Background colour picker
- Density slider (1k–100k lines)
- Line weight in mm
- Opacity (3–255)
- Seed input with step/random buttons

**Randomisation**
- Per-pass RND button
- Randomise All button
- Parameter locking — lock any combination of: colour, ratio X, ratio Y, amplitude, phase, detune

**Presets**
- 8 built-in presets: Lobed, Trefoil, Star, Spiral, Ribbon, Lattice, Rose, Figure-8
- User presets — save/load to localStorage
- JSON export/import for preset sharing

**Undo / Redo**
- 40-state history stack
- Ctrl+Z / Ctrl+Y / Ctrl+Shift+Z keyboard shortcuts

**Export**
- SVG export with per-layer `<g>` elements in real mm coordinates

**UI**
- All displayed values are directly editable (click to type)
- Unit labels shown where applicable (mm, rad)
- Inline progress bar during rendering
