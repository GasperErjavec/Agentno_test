# Recommendations for Re-implementation — KV5687-ZaznavaDrsnika

## Priority Order

These recommendations are ordered by impact. Address them in sequence.

---

## 1. Fix the alarm level swap (CRITICAL — do first)

Before any other changes, correct the crossed wiring between detection workers and the Main orchestrator:
- `inAlarmLevoLevel1` must receive the Level 1 signal
- `inAlarmLevoLevel2` must receive the Level 2 signal
- Same for `inAlarmDesno*`

Verify by triggering each alarm level independently and confirming the semafor shows the correct color.

---

## 2. Centralize all configuration

Move all hardcoded values to a single configuration source (HMI parameters or a config file):

| Parameter | Current value | Make configurable |
|---|---|---|
| PLC IP address | `172.20.1.160:502` | Yes |
| Left camera serial | `K95789557` | Yes |
| Right camera serial | `K95789593` | Yes |
| Simulation image path | `D:\Repository\...` | Yes |
| ROI Level 1 width | `350` px | Yes (already partial via HMI slider) |
| ROI Level 2 width | `800` px | Yes |

---

## 3. Clean up dead code

Remove or isolate:
- Disabled `CheckPresence_PixelAmount` block
- `RandomInteger` usage
- Disabled `SaveImage` block
- Unused `Simulacija` worker (keep in Simulation module only)

If any of these are needed for future debugging, move them to the `Simulation` module explicitly.

---

## 4. Replace magic numbers with named constants

Define at minimum:
```
IMAGE_WIDTH         = 2048
ROI_LEVEL1_WIDTH    = 350
ROI_LEVEL2_WIDTH    = 800
QUEUE_TIMEOUT_MS    = 3000
COLOR_CHROMA_TOL    = 70
```

---

## 5. Standardize naming

Adopt a consistent convention across all files:
- **Language**: English for all identifiers
- **Convention**: CamelCase for worker/function names, snake_case for variables
- **Translations**: Levo → Left, Desno → Right, Semafor → TrafficLight, Drsnik → Slider

---

## 6. Document module responsibilities

Add a one-line header comment to each module:
- `Hardware.avlib` — Beckhoff PLC I/O read/write
- `HMI.avlib` — UI state read/write and display updates
- `Simulation.avlib` — offline image playback for testing

---

## 7. Write a proper README

The new README should include:
- What the system detects and why
- Hardware requirements (cameras, PLC, network)
- How to configure (config file / HMI parameters)
- How to run in simulation mode
- Description of alarm levels and semafor states
