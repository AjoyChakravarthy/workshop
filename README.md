# Workshop

Personal DIY engineering assistant — Claude Code + skills + Git.
Turns project ideas (RC builds, Arduino/ESP32 electronics, mechanical designs)
into buildable plans: BOM, wiring, firmware, OpenSCAD models, staged build plans.

## Setup (10 minutes)

1. **Unzip / clone this repo** somewhere permanent:
   ```bash
   cd ~/code && git init workshop && cd workshop
   # copy these files in, then:
   git add -A && git commit -m "workshop: initial scaffold"
   ```

2. **Fill in `WORKSHOP.md`** — replace every `TODO` with your real printer specs,
   soldering capability, and parts inventory. 15 minutes with your parts drawer
   open. This is the highest-leverage step: the assistant designs around what
   you actually own.

3. **Install the verification tools** (optional but recommended — lets the
   assistant check its own work):
   ```bash
   # OpenSCAD (headless render checks)
   winget install OpenSCAD.OpenSCAD        # Windows
   # arduino-cli (firmware compile checks)
   winget install ArduinoSA.CLI
   arduino-cli core install esp32:esp32
   ```

4. **Open Claude Code in this folder** and say:
   > "New project: [your idea]"

   The `project-blueprint` skill takes over: measurements → feasibility →
   architecture options → artifacts → staged build plan.

## The workflow

```
Idea → Phase 0: measure (calipers!) → Phase 1: feasibility gate
     → Phase 2: architecture tradeoffs (you decide)
     → Phase 3: BOM + SCAD + firmware + wiring
     → Phase 4: build plan with test checkpoints
     → build, film, commit
```

## Improving it

This assistant gets better the same way your PR-review skills did: every time
it makes a real-world mistake (part didn't fit, missed a stall-current check),
tell Claude Code to encode the failure as a rule in the relevant skill and
commit it. Calibrated print tolerances flow back into `WORKSHOP.md`
automatically via the test-coupon protocol.

## Skills included

| Skill | Job |
|---|---|
| `project-blueprint` | Phase-gated planning: measure → feasibility → architecture → artifacts → build plan |
| `openscad-mechanical` | Parametric printed parts with your printer's calibrated tolerances |
| `circuit-design` | Schematics, PCBs, power budgets, PCB-as-chassis pattern, bring-up procedure |
| `firmware-embedded` | ESP32/Arduino code with mandatory failsafe + low-batt behavior, ESP-NOW RC links |
