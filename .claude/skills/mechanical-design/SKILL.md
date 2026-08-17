---
name: mechanical-design
description: Design mechanical parts for DIY builds — chassis, enclosures, mounts, brackets, frames, gears, linkages — fabricated by hand from acrylic/PVC/plastic sheet, aluminum, or wood, OR outsourced to a 3D printing service for complex geometry. ALWAYS use this skill whenever a project needs any physical part made, when the user mentions cutting, drilling, enclosures, mounts, chassis, acrylic, PVC, 3D printing, STL, DXF, or mechanical fit, or when project-blueprint reaches Phase 3 with mechanical artifacts. Routes every part to the cheapest adequate fabrication method — do not default everything to 3D printing.
---

# Mechanical Design

The user has NO 3D printer. Fabrication routes, in order of preference:

1. **Hand fabrication** (default): acrylic / PVC / plastic sheet, aluminum angle, wood — cut, drilled, filed, bent (acrylic bends with heat), solvent-welded or screwed. Cheap, same-day, iterable.
2. **Outsourced 3D printing** (only when geometry demands it): complex curves, integrated features, gears, parts that would need 5+ hand operations. Costs money and days of lead time — minimize part count sent out, and batch all printed parts of a project into ONE order.

Read `WORKSHOP.md` `## Fabrication` for available tools, materials on hand, and the user's chosen print service + its published tolerances.

## Design-for-hand-fabrication rules (the important part)

- **Think in 2D first.** Most hand-fab parts are flat plates + standoffs + brackets. Decompose 3D shapes into stacked/perpendicular flat pieces joined by screws, tabs+slots, or solvent weld.
- Deliverable for flat parts: **1:1 printable PDF cutting/drilling template** (paper template glued to the sheet is the classic method) + a dimensioned drawing (SVG) with every hole position from a single datum edge — never chained dimensions (errors accumulate).
- Hole positions tolerance: assume ±0.5mm hand drilling. Design accordingly: slotted holes for anything needing alignment, oversized holes + washers where position isn't critical.
- Respect material reality: acrylic cracks — no tapping threads into it near edges, minimum 2× material thickness from hole edge to sheet edge, drill with step bits or backing; PVC is forgiving and solvent-welds well; note which material each part should use and why.
- Bends: acrylic strip-heater bends get a marked bend line and a flat pattern with bend allowance ≈ thickness.
- Standard hardware over custom: M3 screws + nuts, threaded standoffs, off-the-shelf hinges/brackets from the hardware store before designing a custom part.

## Design-for-outsourced-printing rules

- Deliverable: parametric OpenSCAD (`.scad`) with named variables + exported STL. Render-verify: `openscad -o /tmp/check.stl part.scad` must exit 0.
- Use the service's published tolerances (WORKSHOP.md), typically better than home FDM: SLS/MJF ±0.3% (min 0.3mm); FDM service ±0.5%. Default clearances: press fit +0.1mm, slide +0.25mm — but state them as variables so one reprint fixes a bad fit.
- Min wall 1.5mm (FDM service) / 1.0mm (SLS). Avoid large flat thin plates (warp + cost) — that's what acrylic is for.
- **Batch the order**: all printed parts for the project in one list with material recommendation per part, and note estimated cost class so the user can decide to redesign a part for hand-fab instead.
- Include lead time in `build-plan.md` scheduling: printed parts get ordered at the same time as PCBs/components, not when the build reaches them.

## Routing decision (apply per part, record in blueprint.md)

| Part looks like | Route |
|---|---|
| Flat plate, box, bracket, panel | Hand-fab (acrylic/PVC template) |
| Simple spacer/standoff | Buy standard hardware |
| Gear, cam, curved shell, snap-fit, one-piece complex bracket | Print service |
| High-load structural | Aluminum angle/sheet or thicker acrylic; print only in SLS nylon |

If a printed part can be redesigned into 2 flat parts + screws with acceptable function, do it and say so.

## Iteration protocol

Fit problem on a hand-fab part → revise the template, note the correction. Fit problem on a printed part → adjust the named clearance variable, log the service's real-world tolerance into WORKSHOP.md `## Calibrated tolerances` so the next order is right the first time.
