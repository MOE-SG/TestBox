# WebEDA Pro

**Schematic & Panel Documentation Workspace**

WebEDA Pro is a self-contained, browser-based schematic capture tool. It runs entirely
client-side from a single `index.html` file — no install, account, build step, or server
required. Every drawing lives on an A3 landscape sheet with a red ASME-style border,
inch-accurate zone references, and a fully editable engineering title block.

---

## Contents

- [Getting Started](#getting-started)
- [Sheet & Title Block](#sheet--title-block)
- [Toolbar Reference](#toolbar-reference)
- [Component Library](#component-library)
- [Element Inspector](#element-inspector)
- [Bill of Materials (BOM)](#bill-of-materials-bom)
- [File Format](#file-format)
- [Controls & Shortcuts](#controls--shortcuts)
- [Browser Support](#browser-support)
- [Known Limitations](#known-limitations)

---

## Getting Started

1. Open `index.html` in any modern desktop browser (double-click it, or serve it from
   any static host — no backend needed).
2. Pick a symbol from the **Engineering Structural Library** on the left, then click on
   the sheet to place it.
3. Wire components together, annotate with text, and fill in the title block on the right.
4. Use the **Output** group to export DXF/JPG or print, and the **Parts** group to
   generate a Bill of Materials.
5. Use **File → Save** to download your work as a `.json` project file, and **File → Open**
   to resume it later.

All project data stays in your browser session and any files you explicitly save —
nothing is uploaded to a server.

---

## Sheet & Title Block

- The drawing sheet is rendered at the true **A3 landscape** aspect ratio (420 mm × 297 mm,
  ≈16.5″ × 11.7″), with a red outer border and inch-accurate zone markers (numbers across
  the top/bottom, letters down the left/right) for callouts like `4-B`.
- The title block (bottom-right) mirrors a standard ASME drawing block:
  - Unspecified tolerances (three fully editable lines)
  - Sheet size designation (e.g. `A3`)
  - Document/drawing number with revision
  - Company name
  - Date, drawn-by, and sheet number (e.g. `1/1`)
- All title block fields are editable live from the **Title Block Fields** panel on the
  right sidebar and update the sheet instantly.

## Toolbar Reference

| Group | Controls | Description |
|---|---|---|
| **File** | New, Open, Save | Start a blank sheet, load a saved `.json` project, or download the current one. |
| **Output** | DXF, JPG, Print | Export a vector DXF, export a raster JPG snapshot, or print an exact, borderless A3 sheet. |
| **Parts** | BOM | Open the auto-generated Bill of Materials. |
| **Insert** | Text, Symbol | Place free-form text annotations or import a raster image as a custom symbol/footprint. |
| **Draw** | Line, Poly, Rect, Circle, Pin | Freehand drawing primitives and terminal pins, all snapped to the active grid. |
| **View** | Grid size, Fit | Change the snap grid (5/10/20 px) or reset zoom to fit the whole sheet. |

The status bar on the right of the header shows the current zoom level, active tool mode,
sheet size, and an **About** (`?`) button with a quick feature/controls summary.

## Component Library

Symbols are organized into three categories in the left sidebar:

**Electrical Components** — Standard Resistor, Variable Resistor, Non-Polar Capacitor,
Polarized Capacitor, Ground (GND), Chassis Ground, Power Source (VCC), Diode, Zener
Diode, Inductor, Transistor (NPN), Switch (SPST), Operational Amplifier.

**Flow Chart Nodes** — Terminal Block, Process Engine, Decision Branch. Useful for
process/logic diagrams alongside or instead of electrical schematics.

**Rack Systems Layout** — 19″ Full Frame Unit, 19″ Half Frame Module, Rackmount
Computer. For enclosure/rack elevation-style layouts.

Each placed part is automatically assigned the next reference designator for its
prefix (e.g. `R1`, `R2`, `C1`, `C2` — numbering is per-prefix, so parts that share a
prefix, like the two capacitor types, never collide).

## Element Inspector

Selecting any placed element opens the **Element Inspector** in the right sidebar:

- **Designator Name** — the reference designator (or annotation text, for text objects).
- **Value / Rating** — free-text field for a component's value (e.g. `10k`, `100nF`,
  `1N4148`). This feeds directly into the Bill of Materials.
- **Show Label** — toggles visibility of the designator/value label on the sheet.
- **Pin Terminal Configuration** — for components with pins, lets you fine-tune each
  pin's position.
- **Element Stroke Color** — applies to the currently selected element, or to the next
  element you draw/place if nothing is selected.

## Bill of Materials (BOM)

Click **📋 BOM** in the Parts group to open a live-generated BOM:

- Components are grouped by **type + value**, so identical parts (same type and same
  Value/Rating) collapse into a single row with a combined quantity and a sorted list
  of reference designators (e.g. `R1, R2, R5`).
- Columns: Item #, Qty, Reference Designator(s), Description, Value/Rating, Category.
- **Export CSV** downloads the table as `<DocumentID>_BOM.csv` for use in Excel or any
  parts/ERP system.
- **Print BOM** opens a clean, separate A4 print layout with its own header (company,
  document ID, revision, date, sheet).

## File Format

**Save/Open** uses a plain JSON structure (`system_schema.json` on save) containing:

```jsonc
{
  "titleBlock": { "company": "...", "docId": "...", "rev": "...", "date": "...", "drawnBy": "...", "sheet": "...", "size": "...", "tol1": "...", "tol2": "...", "tol3": "..." },
  "components": [ /* placed parts: type, x, y, rotation, refDes, value, color, pins, ... */ ],
  "wires": [ /* net connections */ ],
  "customImages": [ /* imported raster symbols */ ],
  "annotations": [ /* free-form text */ ],
  "shapes": [ /* lines, polylines, rectangles, circles */ ]
}
```

This makes projects easy to version-control, diff, or script against outside the app.

## Controls & Shortcuts

- **Wires/Nets** — select a wire directly, or drag a marquee box around it and press
  <kbd>Backspace</kbd> to delete.
- **Move Pins** — click a component, then drag any red terminal pin dot to reposition it.
- **Marquee Box** — drag across empty canvas space to sweep-select multiple parts or
  custom-drawn lines at once.
- **Spacebar** — rotate the current selection 90°.

## Browser Support

Any current desktop browser with Canvas2D support (Chrome, Edge, Firefox, Safari).
Printing uses `@page` sizing for exact, borderless A3 output — tested in Chromium-based
browsers; other browsers should honor the same CSS but may vary slightly in print preview.

## Known Limitations

- No electrical rule checking (ERC) or netlist export — this is a documentation/drafting
  tool, not a full EDA suite with simulation.
- Imported symbols/footprints are raster images (PNG/JPG), not editable vector symbols.
- Multi-sheet projects are not yet supported; each project file represents one A3 sheet.

---

*Built for SOE MOE ENTERPRISES.*
