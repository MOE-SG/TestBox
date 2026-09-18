# Moe's Test Box

**A browser-based panel layout tool for electronics enclosures — design, dimension, and document a panel entirely offline, in one HTML file.**

Moe's Test Box lets you lay out drill patterns, cutouts, and mounting holes on any face of an enclosure (die-cast box, extruded case, or 19" rack panel), then take that design straight through to production:

- **DXF** for CNC machining
- **A fully dimensioned A3 drawing sheet** with a title block, for the shop floor
- **A bill of materials** (printable, and one-click Excel export) for purchasing

No install, no server, no account. Open `index.html` in a browser and start designing.

---

## Quick start

1. Open `index.html` in any modern desktop or mobile browser.
2. Pick an enclosure from the **Enclosure Library** (or add your own from a datasheet with **+ Enclosure**).
3. Pick a panel face from the **PANEL** dropdown.
4. Drag components from the **Component Library** onto the panel, or use the canvas tools (**Hole**, **Rect Cut**) to draw a custom cutout.
5. Export what you need from the toolbar:
   - **⬇ DXF This Panel / All Panels** — CAD file for machining
   - **🖨 Print A3** — a dimensioned drawing sheet, ready to print or save as PDF
   - **📋 Generate BOM** — a parts list, printable or exportable to Excel
   - **💾 Save JSON** — the whole project, to reopen and keep editing later

On a phone or narrow window, the toolbar wraps and the workspace collapses into three tabs — **Library**, **Canvas**, **Properties** — so the same tool works on a small screen without a redesign of your workflow.

---

## Features

### Design
- **Enclosure library** — Hammond 1590/1455 die-cast series, EIA-310 rack panels, and custom enclosures defined from a datasheet (external dimensions, wall thickness, panel faces).
- **Component library** — BNC, XLR, RCA, IEC power inlets, D-Sub (9/15/25), USB-A/C, HDMI, RJ45, potentiometers, switches, LEDs, rivnuts, standoffs, DIN rail — organized by category, with a manager for adding your own.
- **Shape types** — circle, clearance hole, rivnut hole, rectangle, rounded rectangle, slot, D-cut, double D-cut, and compound (any combination — e.g. a connector body plus its mounting holes as one placed item).
- **Custom components** — build a new library entry from a datasheet dimension (diameter, width × height, or diameter + flat offset), save it into your own JSON-backed library.
- **Snap-to-grid, pan/zoom, multi-panel tabs** for enclosures with more than one machinable face.
- **Light / dark theme.**
- **Touch support** — tap to select, drag to move, double-tap to edit; works on phones and tablets, not just mouse/trackpad.

### Output

| Output | What it gives you |
|---|---|
| **DXF** | AutoCAD R12 (AC1009), opens in FreeCAD/QCAD/LibreCAD/AutoCAD, layered as OUTLINE / CUTOUT / HOLE / RIVNUT / MOUNTING / DIM / REFERENCE. |
| **A3 print sheet** | ISO A3 sheet (landscape or portrait), a red ASME-style title block (tolerances, drawing no., rev, company, date, drawn by, sheet), per-inch zone lettering/numbering on the border, overall panel dimensions, optional per-feature X/Y dimensioning, and optional enlarged **feature detail drawings** for anything a simple diameter callout can't fully describe (a D-cut's flat depth, a double-D-cut's across-flats width, a compound connector's bolt-circle pattern) — each with a lettered balloon linking it back to the panel. |
| **Bill of materials** | One line per distinct part, matched against the component library by name where possible. Connectors and switches are listed as supplied. Plain and compound mounting holes are recognized by their label (`M3`, `M4`...) or clearance diameter and get a matching screw + nut + flat washer — or a screw only, for rivnut holes. Available as a printable page or a downloaded `.xlsx`. |
| **JSON project file** | Full design state (enclosures used, all placed items across all panels) — reopen and keep editing later. |

### Standards referenced
- DXF: AutoCAD R12 (AC1009)
- Rack panels: EIA-310-D / IEC 60297-3
- BNC cutout: MIL-STD D-cut, Ø9.7mm / 8.7mm flat
- Banana socket: Ø8.33mm double D-cut, WAF 6.35mm
- DIN rail: EN 60715, 35mm slot pattern
- Drawing sheet: ISO A3; tolerance block and zone lettering styled after ASME Y14.1

---

## Installing it as an app

Moe's Test Box is an installable Progressive Web App. When it's hosted over HTTPS (or on `localhost`):

- **Desktop Chrome/Edge** — click the **⬇ Install App** button in the toolbar (or the install icon in the address bar). It opens in its own window, with its own icon, like a native app.
- **Android Chrome** — the **⬇ Install App** button triggers the same "Add to Home Screen" install prompt.
- **iOS/iPadOS Safari** — Safari doesn't support the automatic install prompt; use **Share → Add to Home Screen** instead. It still gets a proper icon and launches full-screen.

Once installed, the app shell (the tool itself) is cached by a service worker, so it keeps working with no internet connection at all — the same offline guarantee described above, now available without needing a browser tab open.

**This requires hosting the whole folder together** (`index.html`, `manifest.json`, `sw.js`, `icon-192.png`, `icon-512.png`) **over HTTPS** — installability is a browser security requirement, not something this app can do on its own. Opening `index.html` directly as a local file (`file://`) still works perfectly as a design tool, it just won't offer the Install button, since browsers don't allow service workers or install prompts outside a real HTTPS origin (`localhost` is the one exception, useful for testing).

---

## Offline behavior

Everything runs client-side with **no network calls**, with one deliberate exception: the **Excel BOM export** loads the SheetJS library from a CDN the first time you use it, so it doesn't add weight to the tool for people who never need `.xlsx`. Every other feature — including the printable BOM, DXF export, and the A3 drawing sheet — works with no internet connection at all.

No data ever leaves your browser. There is no backend.

---

## File layout

The application itself is a **single self-contained HTML file** (`index.html`) — all markup, styling, and logic live in one place by design, so it can be opened directly or dropped into any file share without a build step. A handful of small sibling files add installability on top of that:

```
index.html      — the entire application (works alone — just open it)
manifest.json   — PWA manifest (name, icons, theme color, display mode)
sw.js           — service worker: caches the app shell for offline use once installed
icon-192.png    — app icon (192×192)
icon-512.png    — app icon (512×512)
README.md       — this file
```

Deploy all five app files together (same folder) for full installability. If you only need the design tool itself with no install/offline-as-an-app capability, `index.html` alone is still fully functional.

---

## Browser support

Any current desktop or mobile browser with canvas and ES2017+ support (Chrome, Edge, Firefox, Safari). Printing to PDF uses the browser's native print dialog — no plugin required. Install/offline support requires a browser with service worker support (all current major browsers) served over HTTPS.

---

## Limitations worth knowing

- **Screw length isn't calculated.** The BOM lists the right screw/nut/washer *sizes*, but length depends on your panel/stack thickness, which is a physical decision left to you — verify before ordering.
- **Geometry-only matching has edge cases.** Parts placed from the component library carry their name automatically and match the BOM/detail-drawing lookups exactly. A manually drawn shape with no library link falls back to matching by geometry (diameter/dimensions) alone, which can misidentify it if another library part happens to share the exact same size.
- **DXF is R12-level geometry** (lines, arcs, circles, polylines) — no parametric features, no 3D.

---

## License

MIT License

Copyright (c) 2026 Soe Moe

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
