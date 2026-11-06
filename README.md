# CNC Mill Visualizer

A single-file G-code simulator for a Haas-style mill. Open `index.html` in a browser, no build step.

## What changed in this version

- **G-code engine**: G00, G01, G02, G03 (I/J/K and R, all three planes, full circles, helices), G12/G13 circular pockets, and every canned cycle: G73, G74, G76, G77, G81 to G89, with G98/G99, G91, L repeats, modal words, I/J/K pecks, and P in seconds or milliseconds.
- **Next op** button in the transport bar: jumps to the next rapid up or tool change (a rapid up and the tool change after it count as one stop).
- **Stress test** dev tool: `Ctrl+Shift+D`, or *Dev tools* in the sidebar. It builds, runs and checks ten programs a second against the real parser, with a lamp per kind and a path preview. Use *Burst* to run 20 of every kind at once. Failures can be loaded straight into the editor.
- Every sample loads with matching cutters and holders.
- Tooltips are drawn on one page-level card that never overlaps its control or leaves the window.
- One design language throughout (Inter, outlined controls, flat dark HUD cards).

## Stress test, headless

`DevStress.batch(n)` runs `n` cases of each kind and returns `{ cases, passed, failed }`. The same seed always builds the same programs, so a failing seed can be reproduced from the panel.
