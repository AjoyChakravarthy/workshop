---
name: firmware-embedded
description: Write firmware for Arduino, ESP32, and embedded DIY projects — motor control, RC links (ESP-NOW/BLE), sensors, battery management, remote controls. ALWAYS use this skill when a project needs any microcontroller code, when the user mentions Arduino, ESP32, sketch, firmware, PlatformIO, or flashing, or when project-blueprint Phase 3 generates a firmware/ folder. Enforces non-blocking patterns and safety behaviors (failsafe, low-battery cutoff) that must never be omitted in RC builds.
---

# Embedded Firmware

Firmware for the user's builds: Arduino-framework C++ (ESP32 family preferred, per `WORKSHOP.md` preferences). The user is a professional TypeScript developer — write clean, well-structured code and explain C++-isms briefly when they differ from TS intuition, but don't over-explain programming basics.

## Project structure

```
firmware/
├── car/            # or device/
│   └── car.ino     # plus .h/.cpp modules if >200 lines
├── remote/
│   └── remote.ino
└── shared/
    └── protocol.h  # packet structs shared by both ends — single source of truth
```

Pin definitions in one block at the top, matching the pin-map table in `blueprint.md` exactly (same names). If one changes, change both.

## Non-negotiable patterns

- **Non-blocking always**: `millis()` scheduling or simple state machines. `delay()` only in `setup()`. RC control loops target ≤20ms cycle.
- **Failsafe is mandatory for anything that moves**: if no valid packet within 300ms (tune per project), stop motors and center steering. This is written first, not last.
- **Low-battery behavior**: read pack voltage via divider; below cutoff (3.3V/cell default), limit throttle then stop; blink status LED pattern. Never let firmware run a LiPo flat.
- **Watch the watchdog**: on ESP32, never starve the idle task; no busy-wait loops.
- **PWM via `ledc`** on ESP32 (not `analogWrite` semantics from AVR); note channel/timer allocation in comments.
- Struct-based binary packets over ESP-NOW with a `uint8_t version` field and checksum; both ends include `shared/protocol.h`.
- Serial debug behind `#define DEBUG` — off in release so timing is honest.

## ESP-NOW RC link recipe (default for remotes)

- Remote reads inputs (ADC joystick w/ deadzone + calibration stored in NVS), sends packet at 50Hz.
- Car ACKs with telemetry packet (battery mV, RSSI) at 5Hz — remote shows low-batt.
- Pairing: fixed MACs compiled in for v1 (printed in `blueprint.md`); broadcast-pairing mode only if user asks.
- Channel pinned; note WiFi-coexistence caveat if the device also serves WiFi.

## Quality gates

1. Must compile before delivery: `arduino-cli compile --fqbn <board> path` when arduino-cli is available; otherwise state clearly the build is unverified.
2. Every magic number is a named constant with unit in the name (`STEER_PWM_FREQ_HZ`).
3. `build-plan.md` gets a firmware bring-up section: flash blink test → serial hello → bench-test each actuator with wheels off ground → range test → failsafe test (turn remote off while driving — REQUIRED checkpoint).
4. State machine sketches (charge states, drive modes) documented as a Mermaid diagram in blueprint.md.

## Calibration & tuning

Expose tunables (deadzone, expo, max throttle, trim) as a `Config` struct persisted in NVS/EEPROM, adjustable over serial menu — so the user tunes without reflashing mid-build-video.
