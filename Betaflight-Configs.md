# Betaflight Configuration Guide (Comprehensive)

> Goal: take a freshly-built quad from **first USB plug-in** to a **safe first hover**, with enough depth to cover common setups (CRSF/ELRS, analog/digital VTX, GPS Rescue, etc.).  
> **Safety rule:** do *all* motor/receiver testing with **props OFF** until the very end.

---

## 0) Assumptions & prerequisites

### Hardware assumptions
- Quad built and smoke-checked (no dead shorts at XT leads).
- FC/ESC stack wired correctly (including any ESC telemetry wire).
- Receiver installed (CRSF or ELRS; other serial RX types are similar).
- Video system installed (Analog VTX+camera, or digital air unit).
- Optional GPS installed (recommended for long-range).

### Software / drivers
- Use the **Betaflight App (PWA)** if you’re on newer releases (the project moved to an online “Betaflight App”).  
  - Betaflight App: `https://app.betaflight.com`
  - Old “Configurator” downloads: GitHub releases.
- Windows users: ensure USB VCP drivers are installed if your FC needs them (many modern FCs work out of the box).

### Notes about versions
Betaflight continues to evolve (tabs/features move slightly). This guide is written to match “modern Betaflight” and the official docs.

---

## 1) First connection & baseline backup (do this before changing anything)

1. **Props OFF**
2. Connect FC to PC via USB.
3. Open Betaflight App.
4. Connect to the COM port.
5. Go to **CLI**:
   - Run: `diff all`
   - Copy/paste into a text file as your “factory baseline”
6. (Optional but recommended) Save a full backup:
   - `dump all` (bigger, but complete)

> Why: you want a clean rollback and a record of defaults.

---

## 2) Firmware (only if needed)

### When you should flash
- You want a newer major release.
- You need to match a known-good version for a specific target or feature.
- Your FC came with unusual firmware or corrupted settings.

### When you should NOT flash
- Everything connects and you’re not chasing a specific feature.
- You’re building a “known working” stack and want to reduce variables.

### Flashing workflow (high level)
1. Firmware Flasher tab:
   - Select correct **Target**
   - Choose version
2. Enable **Full chip erase** for a clean slate (typical for major changes).
3. Flash.
4. Reconnect and restore only what you *intend* to restore (avoid pasting random dumps from different versions/targets).

---

## 3) Setup tab: board alignment & accelerometer

### Board alignment
- Ensure the **arrow on the FC** points forward.
- If the FC is mounted rotated, set the appropriate yaw rotation in **Configuration** (or alignment settings) so the 3D model in Setup moves correctly.

### Accelerometer
- If you plan to use **Angle / Horizon** modes, calibrate it on a level surface.
- If you only fly Acro, accelerometer isn’t strictly required, but keeping it calibrated doesn’t hurt.

---

## 4) Ports tab: define your UART map (this is where most builds succeed or fail)

Go to **Ports** and configure features per UART.

Typical examples:

### Example A: CRSF/ELRS receiver + analog VTX (SmartAudio/Tramp) + GPS
- UART1: **Serial RX** (Receiver)
- UART2: **VTX peripheral** = SmartAudio or Tramp
- UART3: **GPS** (GPS sensor)
- UARTx: **ESC telemetry** (if separate wire and your ESC supports it)

### Example B: Digital (MSP DisplayPort) + receiver + GPS
- UART1: **Serial RX**
- UART2: **MSP** (for VTX/OSD depending on system)
- UART3: **GPS**

**Rules of thumb**
- Only enable **Serial RX** on the UART that physically has your RX signal wires.
- For analog VTX control: enable **SmartAudio** or **Tramp** on the UART your VTX control wire is soldered to.
- For digital systems: follow the manufacturer’s wiring guide; many use an MSP link.

Save after making changes.

---

## 5) Configuration tab: core flight settings

### Mixer
- Quad X (most typical).

### ESC/motor protocol
- Modern default is often **DShot** (e.g., DShot300/600). Use what your ESC and build can reliably handle.

### Motor idle / airmode strategy
- Common practice: configure **Airmode** on a switch (Modes tab) rather than “always on,” especially while you’re learning.  
  (Plenty of pilots still run permanent Airmode; choose intentionally.)

### Other common toggles
- OSD: ensure it’s enabled if you want an on-screen display.
- Features like telemetry, dynamic filter, RPM filtering etc. depend on your version and hardware. If you’re not sure, start with defaults and revisit after first flights.

Save.

---

## 6) Power & Battery tab: voltage, current, and warnings

Configure these early so the quad doesn’t surprise you:
- Battery voltage type (LiPo / LiHV)
- Voltage meter source and calibration (if needed)
- Current meter source and calibration (if needed)
- Warnings (minimum cell voltage, etc.)

> If your current sensor is wrong, your mAh consumption readings will be wrong—calibrate later after a few packs.

---

## 7) Receiver tab: verify sticks, channels, and endpoints

### Receiver type
- For CRSF/ELRS (serial receivers), make sure:
  - Ports: Serial RX enabled on correct UART
  - Configuration: receiver mode is set appropriately (serial-based receiver)

### Channel map
- Common is **AETR1234** or similar; confirm your radio mapping.

### Endpoints and center
- Center around 1500
- Low around ~1000
- High around ~2000

### Critical: verify the *direction* of each channel
- Roll left/right, pitch forward/back, yaw, throttle.
- If anything is inverted, fix it in the radio or channel settings.

---

## 8) Modes tab: arm, angle, beeper, airmode, and safety switches

At minimum:
- **ARM** on a dedicated switch (not a sticky/temporary control)
- Optional but useful:
  - **BEEPER** (if you have a buzzer)
  - **AIRMODE** on a switch
  - **ANGLE/HORIZON** if you want stabilized modes
  - **FAILSAFE** mode range (used for testing some failsafe behaviors, and for GPS Rescue testing)
  - **GPS RESCUE** mode (if you’re using GPS rescue)

Set ranges carefully and confirm the correct AUX channel.

---

## 9) Failsafe tab: set it deliberately and test it

Failsafe is “what the craft does when it loses control link.”

Common, safer defaults:
- Stage 1: short “hold” window (very brief)
- Stage 2: **Drop** or **GPS Rescue** depending on your build and goals

**If you enable GPS Rescue:** validate it thoroughly and do not treat it as “guaranteed return-to-home.”

### Bench test (props off)
- Arm (without props).
- Turn off transmitter or trigger failsafe switch.
- Confirm the FC responds as expected.

---

## 10) Motors tab: motor order, direction, and ESC features (props OFF)

### Read this twice
- **Props OFF** for all motor tests.
- Keep fingers and loose clothing away.

### Motor order
- Use the Motors tab slider test to confirm:
  - Motor 1 spins in the motor 1 position, etc.
- If the ESC is rotated or wiring is non-standard, you may need:
  - Motor remapping (via CLI) or
  - Reordering motor outputs depending on your target/version.

### Motor direction
- Confirm motor direction matches your chosen prop direction scheme.
- Fix direction via:
  - ESC configuration tool (e.g., BLHeli) OR
  - Swap any two motor wires for that motor (physical method).

---

## 11) OSD tab: essentials (make it usable on first flight)

Recommended OSD elements:
- Battery voltage (and/or per-cell)
- RSSI / LQ (link quality for ELRS/CRSF)
- Timer (arm time)
- Warnings
- Craft name
- GPS sats + home distance (if using GPS)
- Current draw / mAh used (if calibrated)

Save.

---

## 12) Video Transmitter tab: VTX control (Analog) or MSP VTX (Digital)

### Analog VTX + SmartAudio / Tramp
1. Ports tab: enable the correct **peripheral** on the VTX UART (SmartAudio or Tramp).
2. Video Transmitter tab:
   - Configure a **VTX Table** (or load a preset/JSON if your app supports it)
   - Verify bands/channels/power levels match your VTX
3. Verify you can change channel/power via OSD menu.

### MSP VTX / OpenVTX (common in some digital systems)
- Some VTX systems can announce band/channel/power over MSP, reducing the need for manual VTX Tables.

---

## 13) GPS tab + GPS Rescue (optional but powerful)

### GPS basic setup
- Ports tab: enable GPS on the correct UART.
- GPS tab: set protocol (often UBLOX for common modules).
- Confirm satellites count and GPS lock.

### GPS Rescue overview
GPS Rescue is an advanced safety feature that can autonomously fly the craft home during failsafe conditions. It must be tuned, tested, and treated as “better than nothing,” not as DJI-grade RTH.

Key items to configure:
- Minimum satellites
- Rescue angle / speed / altitude behavior
- Landing behavior (if enabled)
- Home point behavior (ensure home is set correctly and consistently)

### Testing strategy (start safe)
1. Verify GPS lock on the bench.
2. Validate rescue engages/disengages as expected on the ground.
3. First airborne tests should be:
   - Line of sight
   - Enough altitude
   - Close range
   - Plenty of battery
4. Be ready to regain control immediately.

---

## 14) PID / Filters / Rates: don’t over-tune before first hover

### First flights goal
- Confirm the quad is stable and controllable.
- Confirm no severe oscillations or hot motors.

### Suggested approach
- Start with defaults or a conservative preset for your frame size.
- Fly, check motor temps, then iterate.

**Red flags**
- Hot motors after short hover
- High-frequency buzzing
- Wobble/oscillation on punch-outs

---

## 15) Presets tab: use curated defaults (but understand what you apply)

Presets can quickly set:
- Receiver protocol settings
- Filter/PID starting points
- OSD recommendations
- VTX setups in some cases

Apply presets cautiously and keep backups (`diff all`) so you can revert.

---

## 16) Final pre-flight validation (before props go on)

Bench (props OFF):
- [ ] Receiver inputs correct and stable
- [ ] Arm switch works as intended
- [ ] Modes switch positions correct
- [ ] Motor order correct
- [ ] Motor direction correct (for your prop direction)
- [ ] Failsafe behavior confirmed
- [ ] VTX power/channel set correctly (legal for your region)
- [ ] OSD readable and includes key telemetry

Then:
- Install props (correct direction and tight)
- Do a short hover test in a safe area
- Check motor temps
- Review blackbox/logs later if you’re troubleshooting

---

## References
Official Betaflight docs (recommended starting points):
- Setup Guide: https://betaflight.com/docs/wiki/getting-started/setup-guide
- Ports Tab: https://www.betaflight.com/docs/wiki/app/ports-tab
- Configuration Tab: https://www.betaflight.com/docs/wiki/app/configuration-tab
- Modes Tab: https://www.betaflight.com/docs/wiki/app/auxiliary-tab
- Motors Tab: https://betaflight.com/docs/wiki/app/motors-tab
- Failsafe Guide: https://betaflight.com/docs/wiki/guides/current/Failsafe
- VTX Tables: https://betaflight.com/docs/wiki/guides/current/VTX-Tables
- VTX overview: https://betaflight.com/docs/wiki/getting-started/hardware/vtx
- GPS Rescue (guide): https://betaflight.com/docs/wiki/guides/current/GPS-Rescue-v4-5

Companion “practical” build references:
- Oscar Liang VTX Tables (practical walkthrough): https://oscarliang.com/smartaudio-tramp-vtx-control-vtxtables/
- Oscar Liang GPS Rescue walkthrough: https://oscarliang.com/setup-gps-rescue-mode-betaflight/
