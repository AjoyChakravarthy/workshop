# WORKSHOP.md — Ajo's Workshop Profile

> Single source of truth about my tools, materials, and capabilities.
> Every skill reads this before designing anything. Keep it updated.
> Lines marked `TODO` need my real values — fill what you know, add the rest over time.

## Fabrication

### Hand tools & methods
- Cutting: TODO (hacksaw? dremel/rotary tool? jigsaw? score-and-snap for acrylic?)
- Drilling: TODO (hand drill? drill press? bit sizes owned?)
- Finishing: files, sandpaper — TODO what's on hand
- Joining: TODO (solvent cement for acrylic/PVC? epoxy? screws + taps?)
- Bending: TODO (heat gun / strip heater for acrylic?)

### Materials on hand / easily available locally
- TODO (e.g., 3mm clear acrylic sheet, 5mm PVC foam board, aluminum angle,
  plywood scraps — note thicknesses)

### 3D printing service
- Service I use: TODO (e.g., Robu 3D printing, local shop, Craftbot service —
  note material options: PLA/ABS/SLS nylon/resin)
- Typical cost + lead time to Kayamkulam: TODO (e.g., ₹8/gram PLA, 5-7 days)
- Published tolerance: TODO (ask the service; default assumption ±0.3-0.5%)

## Calibrated tolerances

> Updated as real parts come back from the service / off the bench.

| Fit | Clearance (per side) | Validated on |
|---|---|---|
| Press fit (printed) | 0.10mm (default — not yet validated) | — |
| Slide fit (printed) | 0.25mm (default — not yet validated) | — |
| Hand-drilled hole positioning | ±0.5mm assumed | — |

## Electronics bench

- Soldering: TODO (iron model? hot air? flux/wick on hand?)
- Smallest package comfortable hand-soldering: TODO (0805? SOIC? QFN?)
- Test gear: TODO (multimeter? bench PSU with current limit? USB power meter? scope?)
- PCB route: TODO (JLCPCB/PCBWay? bare or assembled? typical turnaround to Kerala?)
- Perfboard/protoboard stock: TODO

## Parts & purchasing

> I buy components per-project, so the BOM in every blueprint is a real
> shopping list. Skills must include supplier, price estimate, and lead time
> per line item, and build-plan.md must schedule ordering BEFORE the build
> stage that needs the parts.

- Preferred suppliers: TODO (e.g., Robu.in ~3-5 days, Amazon.in ~2 days,
  AliExpress ~3 weeks — cheap but slow, only for non-blocking extras)
- Standing stock I do keep: TODO (resistor kit? jumper wires? M3 hardware?
  JST connectors? heat shrink? — even a rough list helps)
- Budget comfort per project: TODO (e.g., ₹500 casual / ₹3000 serious build)
- Salvage sources: TODO (old electronics I strip for motors/parts?)

## Preferences

- Default MCU family: TODO (suggestion: ESP32 family unless project says otherwise)
- Default radio link: ESP-NOW between ESP32s unless a reason not to
- Default connectors: TODO (e.g., JST-PH for battery, Dupont for signals)
- Design style: parametric everything; dimensioned drawings from a single datum;
  I want to tweak variables/templates, not redraw

## Safety notes

- No mains-voltage projects without explicit discussion
- All embedded LiPos get protection circuits — no exceptions
- Flag anything needing ventilation (soldering, solvent cement, spray paint, acrylic heat-bending)

## YouTube

- Channel: MonkeyWrenchMechanicManiac
- Every project's blueprint.md Decisions log doubles as script material
- Prefer builds with a clear "reveal" moment for video structure
