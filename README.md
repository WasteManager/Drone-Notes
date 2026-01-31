A curated set of **practical, build-day** notes for assembling and configuring FPV drones—focused on:
- A repeatable **hardware build flow** (frame → wiring → smoke check)
- A comprehensive **Betaflight configuration** flow (ports/UARTs → receiver → motors → failsafe → VTX/GPS)
- Receiver ecosystem notes and update workflows for **CRSF** and **ELRS**

> These docs are written to be used **at the bench**, not as theory. Expect checklists, “why’s,” and common gotchas.

---

## Contents

### Build (Hardware)
- **General Build Guide**  
  `general_fpv_drone_build_guide.md`  
  A full start-to-finish build flow with practical considerations and caveats.

- **Simplified Build Checklist**  
  `simplified_fpv_build_checklist.md`  
  A quick, build-day checklist derived from the build guide.

### Betaflight (Software)
- **Comprehensive Betaflight Configuration Guide**  
  `betaflight_configuration_guide.md`  
  From first USB connect → firmware (optional) → ports/UART mapping → receiver → motors → OSD → VTX → GPS.

- **Betaflight Configuration Checklist**  
  `betaflight_configuration_checklist.md`  
  A step-by-step configuration checklist for fast execution.

### Receivers
#### CRSF (TBS Crossfire)
- **CRSF Flashing & Updating Guide (TBS Tango 2 + CRSF Nano RX Pro)**  
  `crsf_flashing_updating_tango2_nanorxpro.md`  
  Firmware update flow, OTA receiver updates, and Betaflight CRSF nuances.

- **CRSF Update + Betaflight Checklist**  
  `crsf_update_betaflight_checklist.md`

#### ELRS (ExpressLRS)
- **ELRS Flashing & Updating Guide (TX + RX) + Betaflight Nuances**  
  `elrs_flashing_updating_guide.md`  
  Versioning, binding phrase strategy, update methods (WiFi / passthrough / UART), and Betaflight setup.

- **ELRS Update + Betaflight Checklist**  
  `elrs_update_betaflight_checklist.md`

---

## Suggested Workflow (Recommended Order)

### 1) Build the quad (hardware)
1. Read: `general_fpv_drone_build_guide.md`
2. Execute using: `simplified_fpv_build_checklist.md`
3. Perform smoke check + multimeter validation before full power

### 2) Configure Betaflight (software)
1. Follow: `betaflight_configuration_guide.md`
2. Run through: `betaflight_configuration_checklist.md`
3. Do **motor order + direction** checks with **props OFF**

### 3) Receiver-specific updates and setup
- If you’re using Crossfire/CRSF:
  1. `crsf_flashing_updating_tango2_nanorxpro.md`
  2. `crsf_update_betaflight_checklist.md`

- If you’re using ELRS:
  1. `elrs_flashing_updating_guide.md`
  2. `elrs_update_betaflight_checklist.md`

---

## Conventions Used in These Notes

- **Props OFF** means *do not install props* until configuration validation is complete.
- **UART map** = written record of what device is wired to what UART (high leverage for troubleshooting).
- **CRSF in Betaflight** also applies to **ELRS**, because ELRS receivers speak CRSF serial protocol to the FC.

---

## Safety Notes (Don’t Skip)

- **Smoke check** before first full-power battery plug-in.
- Never power a VTX without an antenna installed.
- Perform motor tests only with props off.
- Treat GPS Rescue as an “assist,” not a guarantee.

---

## Quick Links (Local Files)

If you prefer a flat list:

- `general_fpv_drone_build_guide.md`
- `simplified_fpv_build_checklist.md`
- `betaflight_configuration_guide.md`
- `betaflight_configuration_checklist.md`
- `crsf_flashing_updating_tango2_nanorxpro.md`
- `crsf_update_betaflight_checklist.md`
- `elrs_flashing_updating_guide.md`
- `elrs_update_betaflight_checklist.md`

---

## License / Attribution

These notes are intended as personal build documentation. Where external references are mentioned inside the documents, follow the original authors’ licensing/terms.
