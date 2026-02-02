# ELRS Flash/Update + Betaflight Checklist

## A) Versioning + options (do first)
- [ ] Decide target ELRS major version (e.g., 3.x)
- [ ] Set correct regulatory domain for your region
- [ ] Choose a binding phrase (recommended) and store it safely
- [ ] Decide initial packet rate (start conservative)

## B) Flash / update TX module
- [ ] ExpressLRS Configurator: select correct TX target
- [ ] Select release version (official stable recommended)
- [ ] Set options (domain, binding phrase, etc.)
- [ ] Flash via supported method (USB/WiFi/other)
- [ ] Confirm TX reports expected firmware version

## C) Flash / update RX
### Option 1: Betaflight Passthrough (installed receiver)
- [ ] Plug FC into USB
- [ ] Do NOT connect to Betaflight Configurator
- [ ] ExpressLRS Configurator: select correct RX target
- [ ] Flash method: Betaflight Passthrough
- [ ] Build & Flash
- [ ] Reboot and verify bind

### Option 2: WiFi update (if supported)
- [ ] Put RX into WiFi mode
- [ ] Connect to RX WiFi
- [ ] Upload firmware / flash via configurator workflow
- [ ] Reboot and verify bind

### Option 3: UART flash (wired)
- [ ] Wire UART/boot pads per receiver target docs
- [ ] Flash via configurator UART method
- [ ] Reboot and verify bind

## D) Betaflight (ELRS = CRSF)
- [ ] Wiring: RX TX → FC RX, RX RX → FC TX, plus power/GND
- [ ] Ports tab: enable **Serial RX** on correct UART
- [ ] Configuration tab:
  - [ ] Receiver mode: **Serial (via UART)**
  - [ ] Provider: **CRSF**
  - [ ] Telemetry enabled (recommended)
- [ ] CLI:
  - [ ] `set serialrx_inverted = off`
  - [ ] `set serialrx_halfduplex = off`
  - [ ] `save`
- [ ] Receiver tab: sticks move correctly; endpoints ~1000/1500/2000
- [ ] Radio: discover sensors (telemetry)
- [ ] OSD: voltage + LQ/RSSI dBm + warnings + timer
- [ ] Failsafe: configured and bench-tested (props off)

## E) Link tuning (after first flights)
- [ ] If LQ dips or failsafe events: lower packet rate / adjust telemetry ratio / check antennas
- [ ] Consider dynamic power if available

****
Bardwell Version

# ExpressLRS (ELRS) Field Checklist

A fast, no-nonsense checklist for the bench/field when you just need things to link and fly.

---

## Pre-flight sanity

- [ ] **Radio battery** charged (or fresh pack)
- [ ] **Quad battery** charged and healthy
- [ ] **Antennas installed** (TX module + RX) and not damaged
- [ ] **Correct model selected** on the radio

---

## Radio module configuration (EdgeTX/OpenTX)

### Internal ELRS module radios
- [ ] **Internal RF = CRSF**

### External ELRS module radios
- [ ] **Internal RF = OFF**
- [ ] **External RF = CRSF**

---

## ELRS Lua quick checks

Open **Tools → ExpressLRS**:

- [ ] Lua page loads (no script error)
- [ ] Module name displays (upper-left)
- [ ] **Packet Rate** set for the flight:
  - [ ] Close freestyle/racing: higher rate (e.g., 250–500 Hz)
  - [ ] Long range: lower rate (e.g., 50–150 Hz)
- [ ] **Switch Mode = WIDE** (recommended)
- [ ] **Telemetry** enabled if using:
  - [ ] Dynamic Power
  - [ ] Link stats / telemetry sensors

---

## Bind / link status

- [ ] Receiver LED indicates **connected** (solid on most RX)
- [ ] Lua shows **Connected** indicator (often “C”)

If NOT connecting:
- [ ] Confirm **TX and RX firmware major versions match**
- [ ] Confirm **binding phrase matches** on TX and RX (requires flashing to correct)
- [ ] Move quad closer and retry (avoid RF noise sources)

---

## Betaflight (if you’re troubleshooting on the bench)

**Ports tab**
- [ ] Receiver UART has **Serial RX enabled**

**Receiver setup**
- [ ] **Protocol = CRSF**
- [ ] Receiver responds in Receiver tab (sticks move)

**Features**
- [ ] **Telemetry enabled** if needed

---

## Arm logic (ELRS quirk)

- [ ] **ARM is on AUX1**
- [ ] **High = armed** (switch up/high triggers ARM range)

---

## Quick Wi-Fi flashing steps (no laptop drivers required)

### RX Wi-Fi mode
- [ ] Power quad, wait ~20–30 seconds unbound → RX enters Wi-Fi mode
- [ ] Connect phone to **ExpressLRS RX**
- [ ] Browse to **http://10.0.0.1**
- [ ] Upload correct **RX** `.bin`

### TX Wi-Fi mode
- [ ] Enable Wi-Fi in Lua (WiFi Connectivity)
- [ ] Connect phone to **ExpressLRS TX**
- [ ] Browse to **http://10.0.0.1**
- [ ] Upload correct **TX** `.bin`

---

## Last checks before launch

- [ ] Correct **failsafe behavior** (props off, disarm on signal loss)
- [ ] RSSI/LQ reasonable at arming distance
- [ ] Props secure + direction correct (if installed)
- [ ] Clear takeoff area
