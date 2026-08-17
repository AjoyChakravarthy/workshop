---
name: circuit-design
description: Design electronic circuits, schematics, custom PCBs, power supplies, motor drivers, battery charging, and wiring for DIY projects. ALWAYS use this skill when a project involves any electronics beyond plugging modules together — custom PCBs, PCB-as-chassis builds, battery-powered devices, motor control, sensor interfacing, or when the user mentions schematic, PCB, KiCad, soldering, or "custom board". Also use for reviewing/debugging existing circuits.
</description>
---

# Circuit Design

Design circuits that survive first power-up. Read `WORKSHOP.md` `## Electronics bench` first — it defines what packages the user can solder and which modules/ICs are already in inventory.

## Design order (always this sequence)

1. **Block diagram** (Mermaid in blueprint.md): power source → regulation → MCU → drivers → loads, with voltage domains labeled.
2. **Power tree with numbers**: every rail's voltage, max current, and source. This is where most DIY designs die — do it before picking parts.
3. **Part selection**: prefer inventory; for new parts, spec exact package and verify hand-solderability against WORKSHOP.md (default assumption: SOIC/0805 OK by hand, QFN needs hotplate/stencil, BGA = order assembled).
4. **Schematic**: as KiCad project when the user wants a PCB, otherwise as a clearly-described netlist + wiring diagram (SVG/Mermaid).
5. **Layout guidance** (if PCB): placement constraints, current paths, antenna keepouts.

## Non-negotiable circuit rules

- 100nF decoupling at every IC VCC pin, physically close; bulk cap (≥10µF) per rail.
- Motor/inductive loads: flyback protection (driver-integrated or explicit diode), separate star-ground point or wide ground pour between motor and logic current paths, bulk cap across motor supply (≥100µF for brushed motors).
- Every MCU: accessible programming header (even 3 test pads) + one status LED. Never design a board you can't flash or debug.
- LiPo charging: MCP73831-class with correct C/2-or-less prog resistor; protection (DW01+FET or protected cell) mandatory for embedded batteries; battery voltage divider to ADC for low-batt cutoff in firmware.
- ESP32 designs: RF antenna keepout (no copper under/around antenna), proper EN pin RC (10k + 1µF), strapping pins checked against boot modes.
- Logic-level check across every domain crossing (3.3V MCU vs 5V peripheral): explicit level shifter or justification why not needed.
- Connectors chosen from WORKSHOP.md preferred list; polarized for anything the user could plug in backwards.

## Deliverables

Into `projects/<name>/electronics/`:
- `schematic.md` — human-readable: every net, every component with value and package, design rationale per block
- KiCad files when requested (`.kicad_pro/.kicad_sch/.kicad_pcb`) — generate via KiCad's Python/CLI tooling when available; otherwise provide the schematic.md so detailed the user can enter it in 20 minutes
- `power-budget.md` — the tree with math, worst-case and typical
- Bring-up section in `build-plan.md`: first power-up ALWAYS through current-limited supply or 100mA fuse; check rails before inserting MCU; smoke-test order listed pin by pin

## PCB-as-chassis pattern (micro builds)

When the board is structural (Hot Wheels class): board outline traced from measured base plate; mounting holes on donor rivet posts; components on one side where possible (the other faces the shell); keep-out zones for wheels/axles/gears drawn as board edges or documented; 1.6mm FR4 default, 0.8mm only if height demands and spans are short.

## Review mode

When reviewing an existing circuit (photo/schematic/description), check in order: power tree math, missing decoupling, flyback, floating inputs, level mismatches, battery safety, then style. Report as a numbered list, severity-tagged (BLOCKER/WARN/NIT) — same format as the user's PR-review skills.
