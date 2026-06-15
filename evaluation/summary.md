# Evaluation Summary — KV5687-ZaznavaDrsnika

## Project Overview

Industrial wire slip detection system for manufacturing quality control. Monitors wire movement on two camera sides (Left/Right) at two severity levels (Level 1: critical, Level 2: warning). Triggers a traffic light (semafor) and siren based on detection results.

**Language**: Adaptive Vision Studio 5.2.10 (visual/dataflow programming)
**Hardware**: 2x Hikrobot GigE cameras, Beckhoff PLC (KL1408/KL2408), optional Optris thermal camera
**Architecture**: Multi-threaded, queue-based dataflow with four parallel workers

---

## Overall Assessment

| Dimension | Rating | Notes |
|---|---|---|
| Architecture | Good | Clean separation into parallel workers and modules |
| Code clarity | Poor | UUID-based port names, magic numbers, mixed languages |
| Correctness | Critical bug | Alarm levels are swapped (see issues.md) |
| Maintainability | Low | Hardcoded values, disabled dead code left in place |
| Modularity | Good | Hardware, HMI, Simulation modules properly separated |
| Documentation | Minimal | README is almost empty, no inline comments |

---

## Key Strengths

- Multi-threaded worker design scales well (Main + Left + Right + Simulation)
- Queue-based inter-thread communication with timeout handling
- Two-level alarm system (warning vs critical) is a solid design
- Module separation (Hardware, HMI, Simulation) is well thought out
- Color thresholding via HSV is appropriate for industrial blob detection
- Simulation module enables offline testing without hardware

---

## Critical Issues

See `issues.md` for the full list. The most severe:

**Alarm levels are inverted.** `inAlarmLevoLevel1` is wired to `inAlarmLevoLevel2` and vice versa — same for the right side. This causes Level 1 (critical) and Level 2 (warning) signals to trigger the wrong semafor state and siren behavior in production.

---

## Recommendations for Re-implementation

See `recommendations.md` for the full list. Key priorities:

1. Fix the alarm level swap before anything else.
2. Extract all magic numbers (pixel dimensions, IP address, paths) to named configuration constants.
3. Replace UUID port names with descriptive identifiers.
4. Remove disabled/dead code blocks.
5. Standardize naming to one language (English) and one convention (snake_case or CamelCase).
6. Add a short module-level comment to each `.avlib` explaining its responsibility.
