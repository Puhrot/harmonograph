# Controls Reference

Full parameter reference for Harmonograph.

---

## Global Panel

| Control | Range | Description |
|---|---|---|
| **Paper size** | A3–A5, portrait/landscape/square | Sets canvas dimensions and SVG output size |
| **Background** | Hex colour | Canvas background colour |
| **Seed** | Integer ≥ 1 | Random seed — same seed + same settings = same output |
| **Density** | 1,000–100,000 | Number of line segments drawn per pass |
| **Line weight** | 0.10–3.00 mm | SVG stroke width in millimetres |
| **Opacity** | 3–255 | Stroke alpha channel value |

---

## Pen Passes

Each pass represents one pen / one layer of the drawing.

### Pass Header

| Element | Description |
|---|---|
| **⠿ drag handle** | Drag to reorder passes |
| **Colour swatch** | Click to open colour picker |
| **● visibility** | Toggle pass visibility |
| **◎ solo** | Solo this pass (hide all others) |
| **⊕ duplicate** | Copy this pass |
| **RND** | Randomise all unlocked parameters for this pass |
| **× remove** | Delete this pass |

### Lock Row

Prevents individual parameters from being changed by RND:

`Col` — colour · `RX` — X ratio · `RY` — Y ratio · `Amp` — amplitude · `Ph` — phase · `Det` — detune

---

## Transform Section

| Control | Range | Description |
|---|---|---|
| **Phase offset** | 0–6.28 rad | Rotates the starting phase of the Lissajous figure |
| **X Offset** | −1 to 1 | Translates the curve horizontally on canvas |
| **Y Offset** | −1 to 1 | Translates the curve vertically on canvas |
| **Scale** | 0.1–2.0 | Scales the curve (1.0 = full canvas) |
| **Rotation** | −180°–180° | Rotates the rendered curve |

---

## Phases Section

Sets how many times the base Lissajous figure is repeated with incrementing phase offsets, creating petal/star patterns.

Options: **1×, 2×, 3×, 4×, 6×, 8×**

---

## Shape Section

### Frequency Ratios

Two grids (X axis, Y axis) each with ratio options:

`1:1` `2:1` `3:1` `4:1` `5:1` `6:1` `7:1` `8:1`
`3:2` `5:2` `7:2`
`4:3` `5:3` `7:3` `8:3`
`5:4` `7:4`
`6:5` `7:5` `8:5`
`7:6` `8:7`

### Detune

| Control | Range | Description |
|---|---|---|
| **Detune X** | 0–0.0125 | Frequency drift on the X oscillators — causes the figure to slowly rotate/spiral |
| **⊞ Asym Y** | toggle | Enables independent Y detune |
| **Detune Y** | 0–0.0125 | (visible when Asym Y is on) Independent drift on Y oscillators |

### Amplitude Decay

| Control | Range | Description |
|---|---|---|
| **Decay X** | 0–10 | Exponential amplitude decay on X — curve shrinks over time on X axis |
| **Decay Y** | 0–10 | Exponential amplitude decay on Y |

> Decay of 0 = no decay (constant amplitude). Higher values = faster collapse.

### Oscillator Tabs (P1–P4)

Four pendulum oscillators, accessible via tabs P1–P4:

| Control | Range | Description |
|---|---|---|
| **Amplitude A1–A4** | 0–1 | Contribution of each oscillator (A1+A2 normalised ≤ 1; A3+A4 normalised ≤ 1) |
| **Phase p1–p4** | 0–6.28 rad | Starting phase of each oscillator |

---

## Math

The harmonograph computes x,y coordinates at each time step t:

```
x = A1·cos(f1·t + p1 + φ) + A2·cos(f2·t + p2)
y = A3·cos(f3·t + p3 + φ) + A4·cos(f4·t + p4)
```

Where:
- `f1, f2` = X frequency ratios × (1 + detuneX)
- `f3, f4` = Y frequency ratios × (1 + detuneY)
- `φ` = phase offset
- Amplitude at time t is multiplied by `exp(-decay × t / totalT)`

---

## Keyboard Shortcuts

| Shortcut | Action |
|---|---|
| `Ctrl+Z` | Undo |
| `Ctrl+Y` or `Ctrl+Shift+Z` | Redo |
