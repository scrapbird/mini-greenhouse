# mini-greenhouse

First woodworking project: outdoor greenhouse cabinet. Plans drafted in FreeCAD, assisted via `freecad-mcp` MCP server.

## Specs (locked)

- External: 2000mm W × 1800mm H × 700mm D
- Frame: timber
- Cladding: polycarbonate
  - Sides + doors + back: **flat** sheet
  - Roof: **corrugated**
- Roof: slanted, high edge at **back**, overhang at front to shed rain clear of doors
- Front: **double doors**, full opening (no centre mullion when open)
- Roof: adjustable/ventable **hatch** for hot-air release
- Shelves: adjustable height, removable
- Floor: raised, slatted (gaps for drainage), strong enough for large full pots

## Conventions

- Units: mm throughout FreeCAD models
- Origin: front-bottom-left corner of external envelope; +X right, +Y back, +Z up
- Naming: `Frame_*`, `Panel_*`, `Door_*`, `Shelf_*`, `Floor_*`, `Roof_*`, `Hatch_*`
- One FreeCAD document per major assembly stage; keep parametric where reasonable (Spreadsheet for global dims)

## Workflow

- User drives FreeCAD GUI; Claude uses `freecad-mcp` to create/edit/inspect objects
- Before geometry changes, confirm intent in chat — this is a learning project, user wants to understand each step
- Prefer Part Design / Sketcher (parametric) over raw Part primitives where it aids future edits

## Style

- Concise. Sacrifice grammar for concision (per global CLAUDE.md).
- No filler. State decisions and tradeoffs in one line.

## Open questions

Tracked in README.md "Open questions" section. Resolve before cutting timber.
