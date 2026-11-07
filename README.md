# CNC Mill Visualizer

A single-file G-code simulator for a Haas-style mill. Open `index.html` in a browser, no build step.

## What changed in this version

- **G-code engine**: G00, G01, G02, G03 (I/J/K and R, all three planes, full circles, helices), G12/G13 circular pockets, and every canned cycle: G73, G74, G76, G77, G81 to G89, with G98/G99, G91, L repeats, modal words, I/J/K pecks, and P in seconds or milliseconds.
- **Next op** button in the transport bar: jumps to the next rapid up or tool change (a rapid up and the tool change after it count as one stop).
- **Stress test** dev tool: `Ctrl+Shift+D`, or *Dev tools* in the sidebar. It builds, runs and checks ten programs a second against the real parser, with a lamp per kind and a path preview. Use *Burst* to run 20 of every kind at once. Failures can be loaded straight into the editor.
- Every sample loads with matching cutters and holders.
- **76 ready-made programs** in the Haas library (facing, slots, pockets, contours, drilling, tapping, boring, chamfer, engraving, 3D and macros), each with its own cutters and holders, and each checked clean by the Setup check.
- **Setup awareness**: the tool, holder, vise, parallels, fixture, table, blank and program are checked against each other every time anything changes (a program, a tool, a slider, a part moved). The *Setup* chip beside the editor header lists each finding (holder nose against the blank and jaws, flute length against cut depth, blank on the parallels and between the jaws, toolpath on the table, cuts reaching the vise) and can stand the blank proud of the jaws for a program that needs it.
- **View cube**: much larger, one per view in a multi-camera layout, and the Orbit view's own cube now sits in the Orbit view instead of doubling up on another view's cube.
- Orbit and Close-up cameras start a lot closer.
- **Collision by real contact**: the Setup check tests the cutter's body against the actual fixed jaw, moving jaw, vise bed and parallels, each with its own thickness and height. Passing over a jaw, or beside it, is fine; only touching it is flagged.
- **Coolant** is drawn as a jet and spray only (no nozzle or hose). Beads left on the blank let go when a cut takes their metal away, all of it clears on a rewind or seek, and it stops at M30 or the end of the program.
- **M07 mist coolant** is a separate simulation from M08 flood: a small air-flow grid rides with the cutter (a nozzle blows air, the spinning cutter drags it round, the tool and holder are solid, and the flow is projected to be divergence-free), and a fog of atomised water droplets is carried by that air, bounces off the cutter and wets the work. M07 turns it on, M09 (or M30) turns it off.
- **View controls** (view toggles, isolate and section, graphics, path types, camera, layout, coolant) are one icon-only strip between the 3D view and the panel below it.
- **Loading screen** is the Shop Tool Sim opening: the lockup writes itself, broken brackets glitch in and join up, an arc runs round it, then it fades once the page is built.
- **Bottom panel**: the mode banner and playback sit in a fixed left column and the view controls scroll on their own on the right, so scrolling one never moves the other. The footer plate is flush with the bottom of the window, with no empty band under it.
- **Status cards** (toasts, program complete, stop banner, mode banner, fit note) share one look: a flat dark card, a bright arc that travels round the outline, and a leading icon that dances.
- Text engraving sweeps a real V-groove at the cutter's point angle (a 60 degree center drill cuts a thin line), instead of stamping a round pit at the start of each stroke.
- Tooltips are drawn on one page-level card that never overlaps its control or leaves the window.
- One design language throughout (Inter, outlined controls, flat dark HUD cards).

## Stress test, headless

`DevStress.batch(n)` runs `n` cases of each kind and returns `{ cases, passed, failed }`. The same seed always builds the same programs, so a failing seed can be reproduced from the panel.
