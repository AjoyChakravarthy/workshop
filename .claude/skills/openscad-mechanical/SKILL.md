---
name: openscad-mechanical
description: Design 3D-printable mechanical parts as parametric OpenSCAD code — chassis, mounts, gears, enclosures, brackets, axle supports, battery clips, remote-control cases. ALWAYS use this skill whenever a project needs any printed or machined part, when the user mentions 3D printing, STL, enclosures, mounts, or mechanical fit, or when project-blueprint reaches Phase 3 with mechanical artifacts. Encodes the user's specific printer tolerances — do not design printed parts from generic assumptions without it.
---

# OpenSCAD Mechanical Design

All mechanical parts are written as parametric OpenSCAD, rendered headless, and tuned to the printer described in `WORKSHOP.md`. Read the `## 3D Printer` section of WORKSHOP.md before designing anything.

## File conventions

- One part per `.scad` file in `projects/<name>/mechanical/`, plus an `assembly.scad` that `use <>`s parts for a fit-check render.
- ALL driving dimensions as named variables at the top with comments and units (mm):

```openscad
// ---- Parameters (mm) ----
cavity_w   = 20.4;  // measured, caliper
cavity_l   = 55.2;  // measured, caliper
wall       = 1.6;   // 4 perimeters @ 0.4 nozzle
fit_clear  = 0.2;   // press-fit clearance, from WORKSHOP.md
slide_clear= 0.4;   // sliding fit
$fn = 64;
```

- Never bury a magic number in geometry. If it appears twice, it's a variable.
- Comment which dimensions are MEASURED vs. CHOSEN.

## Tolerance rules (defaults — override from WORKSHOP.md if user has calibrated values)

| Fit | Clearance per side |
|---|---|
| Press fit (bearing, magnet) | +0.10–0.15mm on hole |
| Snug slide (battery pocket) | +0.20mm |
| Free slide / pivot | +0.30–0.40mm |
| Screw self-tap into plastic (M2) | hole = 1.7mm |
| Screw clearance (M2/M3) | 2.4 / 3.4mm |
| Shaft pivot hole (1mm axle) | 1.2mm |

Vertical holes print undersized: add +0.1mm to horizontal-axis holes, or design teardrop/bridged holes for unsupported horizontals.

## FDM design rules

- Min wall: 2 × nozzle diameter. Min standalone feature: 1mm.
- Overhangs >50° need chamfers or support-free redesign — prefer chamfers (45°) under all floating features.
- Orient the part for strength: layer lines perpendicular to bending load. State intended print orientation in a comment at the top of the file.
- Gears: FDM resolves module ≥ 0.5 reliably (module 0.8+ preferred). For smaller, spec a purchased brass pinion in the BOM instead. Use a proper involute library (BOSL2 or `gears.scad`) — never approximate teeth with triangles.
- Fillet interior corners of load-bearing brackets (min r=1mm).

## Workflow

1. Write/modify the `.scad`.
2. Render check every time: `openscad -o /tmp/part_check.stl part.scad` — must exit 0. If OpenSCAD isn't installed, note it and validate syntax mentally, but say the render is unverified.
3. Report printed-part mass and print-time estimate class (small <30min / medium / large) so the user can plan iterations.
4. For fit-critical parts, generate a **test coupon** first: a minimal print (e.g., just the bearing pocket, 5-minute print) so the user validates tolerance before the 2-hour chassis print. Put coupons in `mechanical/coupons/`.
5. Export instruction: user slices STL with profile noted in WORKSHOP.md.

## Iteration protocol

When the user reports a fit problem ("too tight", "wobbles"), adjust the named clearance variable — not the geometry — and log the calibrated value back into WORKSHOP.md `## Calibrated tolerances` so every future part benefits.
