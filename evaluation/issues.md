# Identified Issues — KV5687-ZaznavaDrsnika

## Severity Levels: CRITICAL / HIGH / MEDIUM / LOW

---

## CRITICAL

### 1. Alarm level inputs are swapped
**Location**: `KV5687-ZaznavaDrsnika.avcode`, lines 613–616
**Description**: The wiring between detection workers and the Main orchestrator has Level 1 and Level 2 alarm signals crossed for both camera sides.
```
inAlarmLevoLevel1  ← receives inAlarmLevoLevel2   (WRONG)
inAlarmLevoLevel2  ← receives inAlarmLevoLevel1   (WRONG)
inAlarmDesnoLevel1 ← receives inAlarmDesnoLevel2  (WRONG)
inAlarmDesnoLevel2 ← receives inAlarmDesnoLevel1  (WRONG)
```
**Impact**: Critical (Level 1) alarms trigger warning (Level 2) behavior and vice versa. Wrong semafor color and incorrect siren activation in production.
**Fix**: Swap the wiring back to match naming.

---

## HIGH

### 2. Hardcoded PLC IP address
**Location**: `Aplication/Modules/Hardware/Hardware.avlib`
**Description**: Beckhoff PLC address `172.20.1.160:502` is hardcoded.
**Impact**: Cannot be changed without modifying source code. Breaks deployment to any other machine or network.
**Fix**: Expose as an `extern public` configuration parameter.

### 3. Hardcoded image path in Simulation module
**Location**: `Aplication/Modules/Simulation/Simulation.avlib`
**Description**: Path `D:\Repository\Application\kv5687-zaznavadrsnika\ZajemAlarm2` is hardcoded.
**Impact**: Simulation mode only works on one specific developer machine.
**Fix**: Expose as a configurable parameter in HMI or config file.

### 4. Hardcoded camera serial numbers
**Location**: `KV5687-ZaznavaDrsnika.avproj`
**Description**: Camera serials `K95789557` (left) and `K95789593` (right) are hardcoded.
**Impact**: Replacing a camera requires source code changes.
**Fix**: Move to a configuration file or HMI parameter.

---

## MEDIUM

### 5. Magic numbers for ROI dimensions
**Location**: Throughout `KV5687-ZaznavaDrsnika.avcode`
**Description**: Values like `350`, `800`, `2048`, `3000` appear without names or comments.
**Impact**: Meaning is unclear; changing one value risks introducing regressions.
**Fix**: Extract to named constants (e.g., `ROI_LEVEL1_WIDTH = 350`).

### 6. UUID-based port/connection names
**Location**: Throughout `.avcode` files
**Description**: Internal connections use GUIDs like `con_f37671ce_7f7c_4d05_88a8_e34d95ddfeac`.
**Impact**: Code is unreadable without the visual editor. Diffs are meaningless.
**Fix**: Rename ports to descriptive identifiers where the tool supports it.

### 7. Dead code left in place
**Location**: `KV5687-ZaznavaDrsnika.avcode`
**Description**: `CheckPresence_PixelAmount`, `RandomInteger`, `SaveImage`, and the `Simulacija` worker are disabled but remain in the file.
**Impact**: Confuses future readers; inflates code size.
**Fix**: Remove or move to a dedicated dev/debug branch.

---

## LOW

### 8. Mixed naming languages (Slovenian/English) and conventions
**Location**: Throughout all files
**Description**: Functions and variables mix Slovenian (`Levo`, `Desno`, `Semafor`, `Drsnik`) with English, and mix CamelCase with snake_case.
**Impact**: Cognitive overhead for non-Slovenian readers; inconsistent codebase.
**Fix**: Pick one language and one convention, apply consistently.

### 9. Folder name typo
**Location**: `/Aplication/` (should be `/Application/`)
**Description**: Minor typo in the top-level source folder name.
**Impact**: Low — just looks unprofessional.
**Fix**: Rename folder in re-implementation.

### 10. Minimal README
**Location**: `README.md`
**Description**: README contains only the project name and a one-line description.
**Impact**: No onboarding information for new developers.
**Fix**: Document purpose, hardware requirements, setup steps, and how to run.
