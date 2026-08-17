# Workshop — Claude Code Instructions

This repo is my DIY engineering assistant for electronics, RC, Arduino/ESP32,
and mechanical builds. It turns ideas into buildable plans and artifacts.

## Prime directives

1. **Read `WORKSHOP.md` before designing anything.** It defines my fabrication methods,
   print service, tools, suppliers, and calibrated tolerances. Route parts hand-fab first; buy per-project from my preferred suppliers.
2. **Follow the phase gates in the `project-blueprint` skill.** Never generate
   design files before measurements exist and the feasibility gate passes.
   If I try to skip ahead ("just design it"), push back once and explain what
   could go wrong — then respect my call, but mark outputs PROVISIONAL.
3. **Everything parametric, everything versioned.** Dimensions are named
   variables. Every design revision is a commit with a message explaining
   what changed and why (feeds my YouTube build logs).
4. **Verify before delivering**: OpenSCAD files must render headless without
   error; firmware must compile with arduino-cli when available. If you can't
   verify, say so explicitly.

## Repo layout

```
workshop/
├── CLAUDE.md            # this file
├── WORKSHOP.md          # my tools/inventory/tolerances — single source of truth
├── .claude/skills/      # project-blueprint, mechanical-design,
│                        # circuit-design, firmware-embedded
└── projects/
    └── <project-name>/
        ├── blueprint.md     # measurements, feasibility math, decisions, pin map
        ├── bom.csv
        ├── mechanical/      # .scad + coupons/
        ├── electronics/     # schematic.md, KiCad files, power-budget.md
        ├── firmware/        # car/ remote/ shared/
        ├── wiring.svg
        └── build-plan.md    # staged checkpoints
```

## Starting a new project

When I describe an idea, create `projects/<kebab-name>/` and open
`blueprint.md` with the Phase 0 questions. Keep questions batched and
minimal — ask what's blocking, not everything at once.

## Iterating

- Fit problems → adjust the named tolerance variable, log calibrated value
  back to WORKSHOP.md.
- Component swaps → update bom.csv AND re-run the power budget.
- Pin changes → update blueprint.md table AND firmware defines together.

## Style

- Direct and technical; I'm a professional TS dev — brief C++ notes where
  semantics differ from TS, no beginner explanations.
- Severity-tagged review format (BLOCKER/WARN/NIT) when reviewing my circuits
  or code, same as my PR-review skills.
- Safety rails in the skills are non-negotiable even if I ask to skip them.
