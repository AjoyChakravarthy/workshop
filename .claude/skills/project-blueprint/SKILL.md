---
name: project-blueprint
description: Turn a DIY project idea (electronics, RC, Arduino/ESP32, mechanical, 3D-printed builds) into a complete, physically buildable plan with BOM, wiring, firmware, and mechanical files. ALWAYS use this skill when the user describes any project idea, build, mod, or "I want to make X" — even casually — including RC conversions, custom PCBs, remotes, robots, tools, or fixtures. Also trigger when the user asks for a plan, blueprint, feasibility check, parts list, or "can this be built". Do NOT skip this skill and design directly; it enforces mandatory feasibility gates that prevent unbuildable designs.
---

# Project Blueprint

Turn project ideas into buildable plans through a mandatory phased process. The phases exist because the most common failure mode is designing something that cannot physically be built: parts that don't fit, motors the driver can't power, gears the printer can't resolve. **Never skip a phase. Never generate design artifacts before Phase 1 passes.**

Read `WORKSHOP.md` in the repo root FIRST — it contains the user's parts inventory, tools, printer constraints, and preferences. Every plan must be checked against it.

## Phase 0 — Measure First (BLOCKING)

Before proposing ANY component or design:

1. Identify every physical constraint the design must live inside (donor shell cavity, enclosure, mounting surface, max weight).
2. Ask the user for **real measurements with calipers** — internal cavity L×W×H, mounting post positions, axle diameters, existing hole spacing, weight of donor parts. Never trust nominal/scale dimensions ("1:64 scale" is not a measurement).
3. Ask for the use context: runtime target, speed/torque expectations, indoor/outdoor, budget ceiling, deadline.
4. Record all answers in `projects/<name>/blueprint.md` under `## Measurements`.

If the user has no calipers or can't measure yet, produce only a *provisional* plan clearly marked `PROVISIONAL — dimensions unverified` and list exactly what to measure before Phase 3.

## Phase 1 — Feasibility Gate (BLOCKING)

Run these checks explicitly and show the math in the blueprint:

- **Volume stack-up**: List every component with its package dimensions. Sketch the placement (text/ASCII or SVG). Sum heights in the tightest axis including wire/connector clearance (+2mm min). PASS only if everything fits with margin.
- **Power budget**: For every load, list nominal AND stall/peak current. Verify: driver continuous rating ≥ stall current, battery C-rating × capacity ≥ total peak draw, regulator dissipation at max load. Compute runtime = capacity ÷ average draw and compare to target.
- **Weight budget** (for anything that moves/flies): total mass vs. motor thrust/torque through the gear ratio. Check wheel torque at stall vs. required.
- **Thermal sanity**: linear regulators dropping >2V at >100mA need a dissipation check.
- **Fabrication routing**: every mechanical part assigned a route — hand-fab (acrylic/PVC/aluminum, the default) vs. outsourced 3D print (complex geometry only) vs. buy-standard-hardware — per the mechanical-design skill's routing table. Every PCB footprint checked against user's soldering capability (see WORKSHOP.md).
- **Procurement feasibility**: the user buys components per-project. Every BOM line needs supplier + price estimate + lead time; total cost checked against budget comfort (WORKSHOP.md); longest lead time flagged as the critical path.
- **Skill/tool gate**: flag any step requiring tools or techniques the user doesn't have (per WORKSHOP.md) and propose alternatives.

If any check FAILS: redesign at the architecture level and re-run the gate. Do not proceed with a failing design and do not silently relax a requirement — surface the tradeoff to the user.

## Phase 2 — Architecture with Tradeoffs

For every major design decision (drive method, steering method, radio protocol, battery chemistry, MCU choice), present **2 options minimum** with pros/cons and a recommendation. The user decides. Decisions get recorded in `blueprint.md` under `## Decisions` with one-line rationale (this is the build's ADR log — also feeds YouTube scripts).

Prefer parts from the user's standing stock and salvage sources (WORKSHOP.md) where they exist, but the default assumption is per-project purchasing: optimize for commonly available, well-documented modules from the user's preferred suppliers over exotic parts with long lead times. When two parts are equivalent, pick the one that arrives faster.

## Phase 3 — Artifacts

Only after Phases 0–2 are complete, generate into `projects/<name>/`:

| File | Content |
|---|---|
| `blueprint.md` | Measurements, feasibility math, decisions, architecture diagram (Mermaid), pin map table |
| `bom.csv` | ref, part, package/size, qty, supplier, est. price (₹), lead time, order-by stage |
| `mechanical/` | Per routing: hand-fab parts get 1:1 PDF cutting templates + dimensioned SVG drawings (single datum edge); outsourced parts get parametric `.scad` + STL, render-checked (`openscad -o /tmp/check.stl file.scad` must succeed), batched into one print order with material notes |
| `firmware/` | Complete sketches (see firmware-embedded skill). Must compile: verify with `arduino-cli compile` if available |
| `wiring.svg` or Mermaid | Every connection, wire gauge for power paths, connector types |
| `build-plan.md` | See Phase 4 |
| `electronics/` | Schematic description + KiCad files if requested (see circuit-design skill) |

Pin maps are single-source-of-truth: the table in `blueprint.md` must match `#define`s in firmware exactly.

## Phase 4 — Build Plan with Test Checkpoints

Write `build-plan.md` as ordered stages where **each stage is verifiable before the next begins**, following this progression:

0. **Order long-lead items immediately** (components, 3D print service batch, PCBs if pre-validated) — the plan schedules ordering so nothing blocks a build stage; short-lead and local items can wait
1. Breadboard the core circuit with parts as they arrive → checkpoint: measured current draw matches budget
2. Bench-test actuators at target load → checkpoint: stall/torque behavior acceptable
3. Fabricate hand-fab parts from templates; dry-fit everything including service-printed parts → checkpoint: fits measurements, mechanisms move freely
4. Perfboard or order PCB (only after breadboard passes)
5. Integration → checkpoint list (range test, runtime test, thermal touch-test)
6. Final assembly + finishing

Each checkpoint states: what to measure, expected value, and what to revisit if it fails. Never let the user order PCBs or commit glue/solder before the relevant checkpoint passes.

## Safety rails (non-negotiable)

- LiPo: always spec a protection circuit or protected cell for embedded batteries; charging circuit must terminate properly (e.g., MCP73831 with correct prog resistor); never spec charging inside a sealed enclosure without venting note.
- Anything mains-powered: state clearly that mains wiring must be reviewed/done per local code; do not provide casual mains designs.
- Motor drivers: spec for stall current, not nominal.
- Flag pinch points, flying-part risks (props, high-RPM), and hot surfaces in `build-plan.md` under `## Safety`.

## Reference patterns

These are worked examples, not the scope — the phases above apply to ANY build
(robots, tools, fixtures, home automation, props, test gear, full-size RC).
As projects complete, append new proven patterns here.

### Micro-scale RC conversions (sub-100mm, Hot Wheels class)
- **PCB-as-chassis**: the board replaces the base plate and carries everything; mounting holes match donor rivet posts.
- **Drive**: 4mm/6×15mm coreless motors (drone spares), 200–500mA stall on 1S; direct or 1-stage printed spur/worm.
- **Steering**: (a) coil+magnet actuator on pivoting front axle with spring return — proportional-ish, tiny; (b) fixed axle + differential tank steer — simplest; (c) 1.7g linear servo — only if ≥8mm height available.
- **Radio**: ESP32-C3/C6 with ESP-NOW to a matching remote — no pairing, <5ms latency, one firmware ecosystem. BLE gamepad as fallback.
- **Power**: 1S 40–100mAh LiPo, MCP73831 + USB-C charge, DRV8837-class driver.
