# Walksnail Avatar System: Firmware Updates + Key Features (Dominator HD + Avatar HD Goggles)

This document covers **Walksnail/Fat Shark Avatar ecosystem** firmware management and common “day-to-day” features:
- Updating **goggles firmware** (Fat Shark **Dominator HD** and Walksnail **Avatar HD Goggles** use the same Avatar firmware family)
- Updating **Walksnail Avatar VTX** (air unit) firmware
- Re-binding after updates
- Practical “how-to” for **Broadcast/Share mode**, **channel selection**, and other high-leverage settings
- Betaflight considerations (MSP + DisplayPort / Canvas OSD)

> **Important:** Firmware procedures and file names change over time. Always pull firmware and procedures from the **official download sources** listed below.

---

## 1) Official sources you should use

### Firmware releases (goggles + VTX)
- Walksnail Hub firmware listing: https://walksnail.app/firmware  *(use this to find the current stable release and release notes)*  
- CaddxFPV / Walksnail Download Center: https://www.caddxfpv.com/pages/download-center  
  - Includes the official **Avatar System Update Procedure** PDF and the “Update Record” changelog files.

**Why:** Walksnail releases multiple firmware tracks; unofficial mirrors often lag or mix files.

---

## 2) Big-picture “why” rules (this prevents 80% of issues)

### Rule A — Keep goggles and VTX firmware aligned
If goggles and VTX firmware are mismatched, you can get:
- warning prompts at boot
- reduced features / instability
- binding quirks

Oscar Liang notes you’ll see mismatch warnings when versions differ and recommends keeping them aligned.  
Reference: https://oscarliang.com/setup-avatar-fpv-system/  (firmware mismatch behavior + practical setup notes)

### Rule B — Use stable power and active cooling during update
The official update procedure warns that **power loss during the ~10 minute update can brick the system**, and recommends using a **fan** to prevent VTX overheating during updates.  
Reference: Avatar System Update Procedure PDF (Caddx) via Download Center. https://www.caddxfpv.com/pages/download-center

### Rule C — Walksnail channel/power settings are set in the goggles UI, not Betaflight
Betaflight’s “Video Transmitter” tab and VTX tables are for **analog** control; Walksnail is controlled in the goggles menu.  
Reference: https://oscarliang.com/setup-avatar-fpv-system/ (practical note on VTX settings being in goggles)

---

## 3) Update the Goggles firmware (Dominator HD / Avatar HD Goggles)

The official procedure uses a firmware file named like:

- `Avatar_Gnd_xx.xx.x.img`  (goggles)

### Steps (official high-level procedure)
1. **Charge the goggles battery fully** (do not update on a low battery).  
2. In the goggles menu, **format the SD card** using the goggles:
   - Settings → Record Set → Format SD Card  
3. Put the `Avatar_Gnd_*.img` file onto the **root** of the SD card.
4. Insert SD card back into the goggles.
5. Power on the goggles and wait until you reach the **Standby** screen.
6. Press-and-hold the **Link** button for ~8 seconds to start update.
7. Wait for completion (beeping during update is normal).
8. After the update, the goggles should return to Standby.
9. **Run “RESET” in goggle settings** (explicitly called out as important in the official procedure).

Source: Avatar System Update Procedure PDF (Caddx). https://www.caddxfpv.com/pages/download-center

### Nuances / gotchas
- **Don’t rename files** unless you are intentionally doing a downgrade strategy (some systems reject “older version” files).
- If update seems “stuck,” verify the file is on the **root** and the SD card is properly formatted by the goggles.

---

## 4) Update the Walksnail Avatar VTX firmware (air unit)

The official procedure uses a firmware file named like:

- `Avatar_Sky_xx.xx.x.img`  (VTX / transmitter)

### Steps (official high-level procedure)
1. Download and unzip the VTX firmware file (`Avatar_Sky_*.img`).
2. Connect the VTX to your computer with the supplied USB cable.
3. **Power the VTX** (if installed on the drone, power the quad with a LiPo; **props off**).
4. The VTX should mount as a USB storage device.
5. Copy `Avatar_Sky_*.img` to the **root** of the VTX storage.
6. Safely eject/disconnect USB.
7. Confirm the VTX LED is blinking green.
8. Press-and-hold the VTX **Link** button for ~8 seconds to start update.
9. During update, LED blinks red; when finished, LED blinks green.

Source: Avatar System Update Procedure PDF (Caddx). https://www.caddxfpv.com/pages/download-center

### Nuances / gotchas
- **Use a fan** pointed at the VTX during the update to avoid overheating (explicitly recommended in the official procedure).
- If the VTX doesn’t mount as storage:
  - try a different USB port/cable
  - ensure the VTX is receiving power from LiPo/regulated supply
- If you accidentally leave firmware files on the VTX storage after a downgrade/experiment, you can confuse future updates—clean up afterward.

---

## 5) Re-bind after updates (goggles ↔ VTX)

After firmware updates, you may need to bind again.

Typical bind flow (practical):
1. Power VTX and goggles.
2. Press the VTX **link** button (LED changes state).
3. Press the goggles **link** button (beeping indicates bind).
4. Wait for a solid “linked” state + video feed.

Reference: Oscar Liang pairing notes. https://oscarliang.com/setup-avatar-fpv-system/

### LED / beep patterns can help
Oscar Liang documents common goggles beeps and VTX LED states for pairing/updating.  
Reference: https://oscarliang.com/setup-avatar-fpv-system/

---

## 6) Changing channels, bitrate, and transmit power (the “real” daily-use items)

Walksnail systems are controlled primarily from the **goggles menu**.

### Channel selection
- Open goggles menu → **Channel**
- Select a channel manually, or use **Auto Scan/Auto** (if available in your firmware)

Walksnail Wiki describes the Channel menu and that “Auto Scan” selects the strongest video signal.  
Reference: https://walksnail.wiki/en/menu

#### Nuance: bitrate/frame rate can limit channel availability
Walksnail Wiki notes that certain higher bitrate/high frame rate settings can limit the number of selectable channels.  
Reference: https://walksnail.wiki/en/menu

**Why:** higher throughput modes can restrict channel choices depending on regional settings and firmware constraints.

### Transmit power
- Goggles menu → **Settings** → Transmit Power (available when VTX is powered and connected)

Walksnail Wiki notes transmit power is enabled when the VTX is powered and connected.  
Reference: https://walksnail.wiki/en/menu

### Standby mode (heat management)
- Goggles menu → **Settings** → Standby Mode

Walksnail Wiki explains Standby Mode places the VTX in low power when not in flight to reduce heat/power.  
Reference: https://walksnail.wiki/en/menu

---

## 7) Broadcast / Share mode (Spectator/Audience mode)

### What it is (and why you’d use it)
Broadcast lets your goggles **transmit your live feed** so another Walksnail pilot can watch on their goggles (Share mode).

- On the pilot goggles: Settings → enable **Broadcast**
- On the spectator goggles: Menu → **Share** → select the available signal

Reference (practical walkthrough): https://oscarliang.com/setup-avatar-fpv-system/  
Reference (menu definition): https://walksnail.wiki/en/menu

### Nuances
- Broadcast may only show as useful when the pilot goggles are already linked to a VTX and showing a feed.
- Treat it as a convenience feature; reliability can depend on environment/interference and firmware maturity.

---

## 8) “FCC hack” / unlock requests (legal + safety reality)

You asked specifically for an “FCC hack.” I can’t provide instructions to bypass regulatory limits or enable restricted channels/power outside what’s legal for your region.

### What I *can* do instead
- Explain **how channel selection and power works** in the goggles menus (above).
- Point you to official documentation and your local rules so you can configure the system **legally**.
- Help you choose antennas, placement, bitrate/framerate, and standby settings to improve performance **without** violating regulations.

If you operate under a specific license or you’re certain higher power/channel access is legal where you fly, use the manufacturer’s official guidance and comply with local RF regulations.

---

## 9) Betaflight: what you must do (Walksnail OSD + control link)

### 9.1 UART setup (Ports tab)
Walksnail uses MSP + DisplayPort for HD OSD.

In Betaflight (example for BF 4.4+), on the UART connected to the Walksnail VTX:
- Set **Peripherals** to: **VTX (MSP + DisplayPort)**  
  (This typically enables MSP automatically)

Reference: Oscar Liang Betaflight 4.4 notes. https://oscarliang.com/setup-avatar-fpv-system/

### 9.2 OSD device setting
Option A (easy): Apply the preset:
- Presets → search “Avatar” → apply “OSD for FPV.WTF, DJI O3, Avatar HD”

Option B (CLI):
- `set osd_displayport_device = MSP`
- `save`

Reference: Oscar Liang BF 4.4 notes. https://oscarliang.com/setup-avatar-fpv-system/

### 9.3 OSD tab
- Set **Video Format** to **HD** so the OSD is sized correctly for the digital feed.

Reference: Oscar Liang OSD notes. https://oscarliang.com/setup-avatar-fpv-system/

### 9.4 What NOT to chase in Betaflight
- Betaflight’s **Video Transmitter** tab and VTX tables are for analog systems.
- Walksnail channel and power are normally set in the **goggles menu** (not BF).

Reference: Oscar Liang clarification. https://oscarliang.com/setup-avatar-fpv-system/

---

## 10) Feature highlights & “how-to” quick hits

### Record settings (goggles vs VTX)
- Set record preferences in goggles:
  - Settings → Record Set
- VTX onboard storage is limited on some units; goggles SD is usually better for long sessions.

Reference: Oscar Liang DVR notes. https://oscarliang.com/setup-avatar-fpv-system/

### Video out (external viewing)
Some goggles support video out via USB-C with adapters; frame-rate compatibility matters (some displays won’t like high frame rates).
Reference: Oscar Liang “Video Out from Goggles USB-C” notes. https://oscarliang.com/setup-avatar-fpv-system/

---

## 11) Walksnail Update + Setup Checklist (printable)

### A) Prep
- [ ] Fully charge goggles battery (do not update on low power)
- [ ] Use known-good SD card and format it in goggles
- [ ] Use a fan for VTX update (cooling)

### B) Update goggles
- [ ] Download correct goggles firmware (`Avatar_Gnd_*.img`)
- [ ] Copy to SD card root
- [ ] Boot goggles to Standby
- [ ] Hold Link ~8 sec
- [ ] Wait for completion
- [ ] Run goggles **RESET** after update

### C) Update VTX
- [ ] Download/unzip correct VTX firmware (`Avatar_Sky_*.img`)
- [ ] Connect VTX to PC via USB
- [ ] Power VTX (props off if on drone)
- [ ] Copy file to VTX storage root
- [ ] Disconnect USB
- [ ] Hold VTX Link ~8 sec
- [ ] Confirm red blink during update, green blink on completion

### D) Post-update
- [ ] Bind goggles ↔ VTX (if needed)
- [ ] Confirm video feed + OSD
- [ ] Set channel/power in goggles menus
- [ ] Enable Standby Mode if you want reduced heat on the bench

### E) Betaflight
- [ ] Ports: VTX UART set to **VTX (MSP + DisplayPort)**
- [ ] OSD displayport device = MSP (preset or CLI)
- [ ] OSD video format = HD
- [ ] Props OFF for motor tests and bench work
