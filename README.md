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
- **Job kit** (Step 3, *Showcase*): one button lays out everything the operator needs for the job in the 3D view, always in the single layout (a multi-camera layout is put back when the kit closes), on a floor with a card under each object: the trial piece (cut the first piece only, then cut the rest of the stock while it machines, shown as a second bar with the cut mark), the parallels, IPA, a Scotch-Brite pad, wipes, WD-40, a dead blow hammer, every tool standing beside its holder (the same tool pictures the Tool builder makes), and the setup sheet on a clipboard with the job's material, blank size, work zero, run time, vise, parallels and a tool table. Click a card to look closer, *Show all* to frame everything again, *Close* or Esc to go back; the camera, Isolate and Section are put back as they were and the program keeps running underneath. Sizes show inches with millimeters in parentheses. The *Before* and *After* blank previews live in the same section now.
- **Playback controls** (transport, last/next op, M01 stop, single block, follow tool, sliders and speeds) are one strip between the 3D view and the panel below it.
- **Loading screen** is the Shop Tool Sim opening: the lockup writes itself, broken brackets glitch in and join up, an arc runs round it, then it fades once the page is built.
- **Bottom panel**: the mode banner and playback sit in a fixed left column and the view controls scroll on their own on the right, so scrolling one never moves the other. The footer plate is flush with the bottom of the window, with no empty band under it.
- **Status cards** (toasts, program complete, stop banner, mode banner, fit note) share one look: a flat dark card, a bright arc that travels round the outline, and a leading icon that dances.
- Text engraving sweeps a real V-groove at the cutter's point angle (a 60 degree center drill cuts a thin line), instead of stamping a round pit at the start of each stroke.
- Tooltips are drawn on one page-level card that never overlaps its control or leaves the window.
- One design language throughout (Inter, outlined controls, flat dark HUD cards).

## Global search

A search field sits at the top left of the page (an icon on a phone). Press `/` or `Ctrl+K` (`Cmd+K` on a Mac) to jump into it from anywhere. It looks through every panel on the page: both sidebars, every section, button and setting, the ready-made samples, the G and M code lookup, the Guide and the Settings dialog, and the help text under them. Results show what each one is and where it lives. Use the arrow keys to move, `Enter` to go, and `Esc` to close. Going to a result opens the dialog, category and section that hold it, scrolls it into view and rings it for a moment. Things that are hidden until something else is set up, such as the part editor before a model is imported, are left out until they can be reached.

## Sidebar steps, empty state and work zero

- **Load a program** is its own group at the top of the left sidebar and is not numbered. The numbered steps are the job itself: **Step 1 Set up the job**, **Step 2 Look around**, **Step 3 Check your work**.
- **Empty state**: while there is no program and no blank, a card in the middle of the view reads "Nothing here? Click here to load a sample or click here to get set up with a custom program". The first link opens the samples, the second opens Step 1. It disappears as soon as anything is loaded.
- **Work zero (origin) setter** in Step 1, under the blank's size: a diagram of the blank with the four corners and the face center. Flip between the **Top face** and **Bottom face** to reach all 8 corners. The chosen point is highlighted, the sim and the G-code positions measure from it, the metal does not move, and a corner stays a corner when the blank is resized. Sizes show inches with millimeters in parentheses.

## Page-wide UX stress test (backtick)

Press the backtick key (`) to open or close the **UX Stress** panel. That key is the only way in: no address parameter, no global function and no button on the page, and while it is closed the page pays for one keydown listener. The key is ignored while you type in a field.

- **Fixed 10 Hz**: one test unit starts every 100 ms, long work is sliced so the page stays smooth, and the Performance tab shows the cadence actually reached.
- **Sandbox copy**: it runs against a hidden copy of the page (an iframe), so your own work is untouched. If the browser blocks that (a `file://` page), choose it or fall back to *This page*, which changes the page's state; reload afterwards.
- **Functional**: finds every button, toggle, slider, number and text field, select, tab, canvas and clickable element, exercises each one (rapid clicks, double clicks, Enter and Space, extreme and hostile values, canvas orbit, pan and zoom), opens hidden panels as they appear, then scenarios (editor typing, find and replace, clear, coolant M07/M08/M09 playback, every select option, a resize sweep) and a seeded random-order fuzz. Thrown errors, console errors and warnings, unhandled rejections, failed resources, controls that do nothing, NaN or undefined on screen and DOM growth are reported.
- **Visual and UX**, at 14 window sizes from 320 px to 2560 px, portrait and landscape: horizontal scroll, text and controls cut off, spilled or truncated labels, overlap, covered controls, tap targets under 44 px (0.46 in, 11.6 mm), contrast, fonts other than Inter, emoji, glyph or non-Font-Awesome icons, filled or outline-less buttons, near-miss alignment, missing names and labels, duplicate ids, keyboard reach and focus visibility, transitions, frame time, long tasks and memory.
- **Report**: Summary (pass or fail per category), Findings (grouped by rule, click one to highlight the element on the page and see where it sits in the window), Elements (pass or fail per control), Viewports, Performance and Log. *JSON* downloads the whole report; *Copy* puts it on the clipboard. The seed is shown, so a fuzz failure can be replayed.

## Stress test, headless

`DevStress.batch(n)` runs `n` cases of each kind and returns `{ cases, passed, failed }`. The same seed always builds the same programs, so a failing seed can be reproduced from the panel.
