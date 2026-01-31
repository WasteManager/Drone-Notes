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
