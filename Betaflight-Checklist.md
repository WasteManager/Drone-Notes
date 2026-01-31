# Betaflight Configuration Checklist (Build-to-First-Flight)

> This is the **quick checklist** version. Keep it open while you configure.  
> **Props OFF** until the very end.

---

## 1) Connect + backup
- [ ] Connect FC over USB
- [ ] CLI: save `diff all` (baseline)
- [ ] (Optional) save `dump all`

---

## 2) Firmware (only if needed)
- [ ] Correct target selected
- [ ] Flash (full chip erase if doing major changes)
- [ ] Reconnect

---

## 3) Setup / alignment
- [ ] Board orientation correct (3D model matches movement)
- [ ] Accelerometer calibrated (if using Angle/Horizon)

---

## 4) Ports tab (UART map)
- [ ] Serial RX enabled on the receiver UART
- [ ] VTX control enabled:
  - [ ] SmartAudio OR Tramp (analog) **on correct UART**
  - [ ] MSP / DisplayPort (digital) **as required**
- [ ] GPS enabled on correct UART (if installed)
- [ ] ESC telemetry enabled (if used)
- [ ] Save

---

## 5) Configuration tab
- [ ] Mixer = Quad X (typical)
- [ ] Motor protocol set (DShot, etc.)
- [ ] OSD enabled (if desired)
- [ ] Save

---

## 6) Power & Battery
- [ ] Voltage meter source correct
- [ ] Warnings set (min cell voltage, etc.)
- [ ] Current meter source set (calibrate later if needed)
- [ ] Save

---

## 7) Receiver
- [ ] Receiver mode correct (serial-based for CRSF/ELRS)
- [ ] Channel map correct
- [ ] Stick directions correct
- [ ] Endpoints ~1000/1500/2000
- [ ] RSSI/LQ present (if configured)

---

## 8) Modes
- [ ] ARM switch set (correct AUX + range)
- [ ] (Optional) AIRMODE on switch
- [ ] (Optional) ANGLE/HORIZON on switch
- [ ] (Optional) BEEPER
- [ ] (Optional) GPS RESCUE mode switch (if using)
- [ ] Save

---

## 9) Failsafe
- [ ] Stage 1/2 configured intentionally
- [ ] Bench test failsafe (props OFF)
- [ ] If using GPS Rescue for failsafe: confirm prerequisites (GPS lock, sats, settings)

---

## 10) Motors tab (props OFF)
- [ ] Motor order verified
- [ ] Motor direction verified (matches prop direction)
- [ ] Fix via BLHeli/ESC tool or swap wires
- [ ] Confirm no abnormal noises

---

## 11) OSD
- [ ] Voltage + timer + warnings visible
- [ ] RSSI/LQ visible
- [ ] GPS elements (if used) visible
- [ ] Save

---

## 12) VTX
### Analog (SmartAudio/Tramp)
- [ ] VTX Table loaded/configured
- [ ] Can change band/channel/power via OSD
- [ ] Pit mode/power level set appropriately

### MSP VTX / Digital
- [ ] MSP link configured as required
- [ ] OSD present and stable

---

## 13) GPS (optional)
- [ ] Satellites count reasonable
- [ ] Home point behavior understood
- [ ] GPS Rescue tested carefully (close range, high altitude, ready to regain control)

---

## 14) Pre-props final checks
- [ ] No unexpected arming
- [ ] Failsafe confirmed
- [ ] Motor test done (props OFF)
- [ ] VTX antenna installed

---

## 15) Props ON + first hover
- [ ] Props correct orientation
- [ ] Short hover test in safe area
- [ ] Check motor temps
- [ ] Re-tighten/inspect after first pack
