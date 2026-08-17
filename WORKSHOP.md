# WORKSHOP.md — Ajo's Workshop Profile

> Single source of truth about my tools, parts, and capabilities.
> Every skill reads this before designing anything. Keep it updated —
> when I buy parts or calibrate tolerances, this file changes.
> Lines marked `TODO` need my real values.

## 3D Printer

- Model: TODO (e.g., Ender 3 V3 SE)
- Bed size: TODO × TODO mm, max height TODO mm
- Nozzle: 0.4mm (TODO if different)
- Materials on hand: TODO (e.g., PLA black/white, PETG clear)
- Slicer + profile: TODO (e.g., OrcaSlicer, 0.2mm quality)
- Known quirks: TODO (e.g., first-layer squish makes holes 0.1 tight)

## Calibrated tolerances

> Skills update this section as we validate real prints via test coupons.

| Fit | Clearance (per side) | Validated on |
|---|---|---|
| Press fit | 0.15mm (default — not yet calibrated) | — |
| Slide fit | 0.30mm (default — not yet calibrated) | — |

## Electronics bench

- Soldering: TODO (e.g., iron only / iron + hot air / hotplate + stencil)
- Smallest package I'm comfortable hand-soldering: TODO (e.g., 0805 + SOIC yes, QFN no)
- Test gear: TODO (multimeter? bench PSU with current limit? oscilloscope? USB power meter?)
- PCB route: TODO (JLCPCB/PCBWay? assembled or bare boards? typical order turnaround to Kerala)

## Parts inventory

### MCUs & dev boards
- TODO (e.g., 2× ESP32 DevKit v1, 3× ESP32-C3 SuperMini, 1× Arduino Nano, 1× Pro Micro)

### Motors & actuators
- TODO (e.g., 4× N20 100RPM, 2× 6×15mm coreless, 5× SG90, 2× 28BYJ-48)

### Drivers & power
- TODO (e.g., 3× DRV8833 breakout, 2× TP4056 module, 5× AMS1117-3.3)

### Batteries
- TODO (e.g., 2× 1S 500mAh LiPo, 4× 18650, 1S 60mAh drone cells ×6)

### Radios & sensors
- TODO (e.g., 2× NRF24L01, MPU6050, HC-SR04 ×3)

### Passives, connectors, hardware
- TODO resistor/cap kits? JST-PH? M2/M3 screw assortment? bearings? 1mm/2mm shaft stock?

## Preferences

- Default radio link: ESP-NOW between ESP32s unless a reason not to
- Default connectors: TODO (e.g., JST-PH 2.0 for battery, Dupont for signals)
- Budget comfort per project: TODO (e.g., ₹500 casual / ₹3000 serious)
- Suppliers I use: TODO (e.g., Robu.in, Amazon.in, AliExpress — note shipping time)
- Design style: parametric everything; I want to tweak variables, not redraw

## Safety notes

- No mains-voltage projects without explicit discussion
- All embedded LiPos get protection circuits — no exceptions
- Flag anything that needs ventilation (soldering, resin, spray paint)

## YouTube

- Channel: MonkeyWrenchMechanicManiac
- Every project's `blueprint.md` Decisions log doubles as script material
- Prefer builds with a clear "reveal" moment for video structure
