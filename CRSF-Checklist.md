# CRSF Update + Betaflight Checklist (Tango 2 + Nano RX Pro)

## A) Update / Flash (TBS Tango 2 + Crossfire)
- [ ] Charge Tango 2 (or ensure stable power)
- [ ] Use known-good USB data cable
- [ ] Backup Tango 2 SD card (recommended for major jumps)
- [ ] Open TBS Agent Desktop/Agent X
- [ ] Update Tango 2 radio firmware (FreedomTX / radio firmware)
- [ ] Update Tango 2 Crossfire (XF) module firmware
- [ ] Reboot radio

## B) Receiver firmware (Nano RX Pro)
- [ ] Power quad/receiver with stable power
- [ ] Put radio into Bind (Crossfire menu)
- [ ] Power Nano RX Pro
- [ ] Accept OTA update prompt (if offered)
- [ ] Keep RX close until update completes
- [ ] Confirm RX links normally afterward

## C) Betaflight (CRSF)
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
- [ ] Receiver tab: sticks move correctly + endpoints ~1000/1500/2000
- [ ] Radio: discover sensors (telemetry)
- [ ] OSD: voltage + LQ/RSSI + warnings + timer
- [ ] Failsafe: configured and bench-tested (props off)
