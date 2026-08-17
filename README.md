# Workshop

Personal DIY engineering assistant — Claude Code + skills + Git.
Turns project ideas (electronics, RC, Arduino/ESP32, robots, tools, mechanical
builds of any kind) into buildable plans: BOM with suppliers and lead times,
wiring, firmware, cutting templates for hand fabrication, 3D print files for
outsourcing, and staged build plans with test checkpoints.

## Setup (10 minutes)

1. **Unzip / clone this repo** somewhere permanent:
   ```bash
   cd ~/code && git init workshop && cd workshop
   # copy these files in, then:
   git add -A && git commit -m "workshop: initial scaffold"
   ```

2. **Fill in `WORKSHOP.md`** — replace every `TODO` with your real info:
   hand tools, materials you can get locally, your 3D print service and its
   lead time, soldering capability, preferred suppliers. This is the
   highest-leverage step: the assistant designs around how you actually build.
   Shortcut: open Claude Code and say **"interview me to fill in WORKSHOP.md"**.

3. **Install verification tools** (optional — lets the assistant check its own work):
   ```bash
   winget install OpenSCAD.OpenSCAD   # render-checks outsourced print files
   winget install ArduinoSA.CLI       # compile-checks firmware
   arduino-cli core install esp32:esp32
   ```

4. **Open Claude Code in this folder** and say:
   > "New project: [your idea]"

## The workflow

```
Idea → Phase 0: measure (calipers!) → Phase 1: feasibility gate
     (fit, power, weight, fabrication routing, cost + lead times)
     → Phase 2: architecture tradeoffs (you decide)
     → Phase 3: BOM/shopping list + templates + print files + firmware + wiring
     → Phase 4: build plan — long-lead orders first, then staged checkpoints
     → build, film, commit
```

Mechanical parts get routed automatically: flat/simple → hand-cut acrylic/PVC
with 1:1 printable templates; genuinely complex → OpenSCAD + STL batched into
one print-service order; simple spacers → standard hardware.

## Improving it

Every time the assistant makes a real-world mistake (part didn't fit, missed a
stall-current check, underestimated a lead time), tell Claude Code to encode
the failure as a rule in the relevant skill and commit it. Validated tolerances
from the print service flow back into `WORKSHOP.md`.

## Skills included

| Skill | Job |
|---|---|
| `project-blueprint` | Phase-gated planning: measure → feasibility (incl. cost/lead-time) → architecture → artifacts → build plan |
| `mechanical-design` | Hand-fab templates (acrylic/PVC, single-datum drawings) + outsourced print files, with routing logic |
| `circuit-design` | Schematics, PCBs, power budgets, safe bring-up procedure |
| `firmware-embedded` | ESP32/Arduino code with mandatory failsafe + low-batt behavior, ESP-NOW RC links |
