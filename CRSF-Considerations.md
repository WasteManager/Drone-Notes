# CRSF Firmware Flashing & Updating Guide (TBS Tango 2 + Crossfire Nano RX Pro)

This guide covers:
- Updating the **radio firmware** on a **TBS Tango 2** (FreedomTX / radio-side firmware)
- Updating the **Crossfire (CRSF) RF firmware** on the Tango 2 (its internal Crossfire module)
- Updating the **Crossfire Nano RX Pro** receiver firmware (typically OTA)
- Betaflight configuration considerations for **CRSF**

> Terminology note: TBS uses “**Agent**” as the umbrella name for their update/config tools (Agent Desktop/Agent X, etc.). The workflow is the same: connect device → manage → update firmware.

---

## 1) What CRSF “is” in practical terms (and why it matters)

### Why CRSF is popular
- **Low latency + strong link** at long range (Crossfire’s original claim to fame).
- **Bidirectional link**: control + telemetry share the CRSF link (so you get LQ/RSSI/telemetry without extra wiring).
- **RC + telemetry are native** in Betaflight via “Serial RX → CRSF”.

### Nuances you need to respect
- **TX and RX firmware should be compatible.** When you update the transmitter’s Crossfire firmware, you usually must update receiver firmware too.
- **Receiver updates are commonly OTA**: the radio will prompt you to update the Nano RX Pro once it detects a mismatch.
- **Stable power matters**: brown-outs mid-update are how you get “mystery bricks.”

---

## 2) Before you update anything (preventable pain)

### Make sure you have
- A good USB-C cable (data-capable, not charge-only).
- A fully charged Tango 2 (or keep it on stable power).
- A stable power source for the quad/receiver when doing OTA updates (don’t do it on a dying LiPo).

### Do this first
- **Backup Tango 2 SD card** contents before major updates.
  - Why: some updates expect specific SD card file layouts; mismatches cause “weird” missing menus or scripts.

---

## 3) Update the Tango 2 (radio firmware / FreedomTX)

### Why you update this first
Radio firmware updates can include bootloader changes and compatibility updates for the Crossfire tools/scripts. If you do the RF first, you’re more likely to chase UI/script issues later.

### Typical workflow
1. Install and open **TBS Agent Desktop/Agent X** on your PC/Mac.
2. Connect the Tango 2 via USB.
3. In Agent, select the device when it appears (green/ready indicator).
4. Go to **Manage** → **Firmware** and update the **radio firmware** (FreedomTX / Tango firmware).
5. Safely eject / reboot radio as instructed.

**Nuance:** If you’re jumping major versions, you may also need to refresh the SD card contents to match that major release (back up first).

---

## 4) Update the Tango 2 Crossfire module firmware (RF firmware)

### Why this matters
The Crossfire module firmware defines the CRSF RF link behavior and features (telemetry behavior, link modes, receiver compatibility).

Workflow (high level):
1. In **TBS Agent**, select the Tango 2 / Crossfire device section.
2. Update the **Crossfire (XF) firmware** for the internal module.
3. Reboot the radio when finished.

---

## 5) Update Crossfire Nano RX Pro firmware (receiver)

### Why receiver updates are “a thing”
Crossfire TX/RX are tightly coupled: after a TX firmware update, the receiver may refuse to link properly until its firmware is updated to match.

### OTA update workflow (most common)
1. Power the Tango 2.
2. Power the quad so the Nano RX Pro is powered.
3. Put the system in **Bind** (if needed).
4. If the receiver firmware is out of date, the radio (or Agent) will prompt an **OTA update**.
5. Accept the update and keep the receiver close to the radio until it completes.

**Nuances**
- Keep the receiver **very close** for OTA updates (reduce packet loss).
- Don’t touch anything. Don’t arm. Don’t power cycle.
- If it fails repeatedly, update via Agent with a wired method (varies by hardware/adapters), but OTA solves most cases.

### “Auto-bind update” nuance
If the receiver was previously bound, you can often trigger the update prompt without pressing the receiver bind button again—just go into bind on the radio, power the receiver, and wait for the prompt.

---

## 6) Binding (Tango 2 ↔ Nano RX Pro)

1. Put Tango 2 Crossfire into **Bind**.
2. Power receiver.
3. Press the Nano RX Pro bind button (only if needed).
4. Confirm solid link indication.

**Why this is tied to firmware**
Rebinding is frequently the “trigger” that causes the system to detect a firmware mismatch and offer OTA update.

---

## 7) Betaflight configuration for CRSF (Crossfire)

### 7.1 Wiring expectations
CRSF uses a full UART pair:
- Receiver **TX → FC RX**
- Receiver **RX → FC TX**
- Plus power and ground.

**Why:** CRSF is a two-way protocol. You need both lines for full telemetry and robust behavior.

### 7.2 Betaflight: Ports tab
- Enable **Serial RX** on the UART you wired to the receiver.

### 7.3 Betaflight: Configuration tab
- Receiver Mode: **Serial (via UART)**
- Serial Receiver Provider: **CRSF**

### 7.4 CLI sanity (important)
CRSF expects:
- `serialrx_inverted = off`
- `serialrx_halfduplex = off`

**Why:** CRSF is uninverted, full duplex. Some pads/ports or prior configs enable inversion/half-duplex for other receiver types; CRSF will act “bound but no sticks” if these are wrong.

### 7.5 Telemetry & radios
- Enable **Telemetry** in Betaflight (recommended).
- On the radio, run **Discover Sensors** (or equivalent) after setup.

**Why:** You want voltage/LQ/RSSI/warnings on the radio and OSD; it’s one of the major advantages of CRSF.

### 7.6 OSD items to show (recommended)
- Battery voltage (and/or per cell)
- Link Quality (LQ) / RSSI
- Warnings
- Timer
- (Optional) mAh consumed if your current sensor is calibrated

---

## 8) Troubleshooting patterns (high signal)

### Symptom: receiver bound (green), telemetry works, but no stick movement in Betaflight
- Ports: Serial RX enabled on the right UART?
- Configuration: Serial receiver + CRSF selected?
- CLI: `serialrx_inverted` and `serialrx_halfduplex` both OFF?
- Wiring: RX↔TX swapped correctly?

### Symptom: receiver won’t update OTA / update fails
- Move receiver closer.
- Ensure stable power.
- Re-initiate bind and try again.
- If persistent, update via Agent (wired) if your hardware allows it.

### Symptom: intermittent failsafe
- Antenna placement and coax damage are common culprits.
- Verify RF mode output power / region configuration per your local rules.

---

## References (recommended)
- TBS Agent overview + usage guide (official)
- TBS Crossfire manual (receiver update OTA, bind flow)
- Oscar Liang: Crossfire / CRSF setup (practical build guidance)
- Betaflight: Receiver + Ports + Failsafe docs

(Links to grab while you write your local SOP)
- TBS Crossfire manual (PDF): https://www.team-blacksheep.com/media/files/tbs-crossfire-manual.pdf
- TBS Agent help article: https://team-blacksheep.freshdesk.com/support/solutions/articles/4000100108-updating-and-using-the-tbs-agent-x
- Oscar Liang Crossfire setup: https://oscarliang.com/crossfire-betaflight/
- Betaflight Setup Guide: https://betaflight.com/docs/wiki/getting-started/setup-guide
- Betaflight Failsafe: https://betaflight.com/docs/wiki/guides/current/Failsafe
