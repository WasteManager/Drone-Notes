# Simplified FPV Drone Build Checklist (Quick Pass)

> Use this as a **build-day checklist**. It assumes you already have the full build guide for explanations and diagrams.

---

## A) Pre-build (bench + parts)

- [ ] Clear bench, good lighting, ventilation
- [ ] Tools ready: iron, solder/flux, hex drivers, cutters/strippers, heatshrink, zip ties/tape
- [ ] **Multimeter** ready (and smoke stopper if you use one)
- [ ] Parts verified: frame, motors, ESC, FC, RX, VTX/camera (or digital unit), props, battery straps, antennas, optional GPS/LED/buzzer
- [ ] Compatibility verified:
  - [ ] Frame stack pattern (20x20 / 30x30) matches FC/ESC
  - [ ] ESC + VTX voltage supports battery (4S/6S)
  - [ ] Receiver type (CRSF/ELRS) supported + free UART available
  - [ ] FPV system wiring plan understood (analog vs digital)

---

## B) Frame + stack (mechanical)

- [ ] Frame assembled (bottom plate/arms/standoffs)
- [ ] Top plate **left off** for access
- [ ] Carbon edges checked (no sharp strap slots / wire rub points)
- [ ] ESC dry-fit in stack location
  - [ ] No contact risk with carbon
  - [ ] Stand-offs/grommets planned (vibration management)
- [ ] FC dry-fit on top of ESC (confirm clearance)

---

## C) ESC power (high current)

- [ ] ESC pads tinned (power + motor pads)
- [ ] Decide XT pigtail placement (rear/side/top) + strain relief plan
- [ ] Choose XT wire gauge (AWG) and cut to length
- [ ] XT pigtail soldered to ESC (+ / -)
- [ ] Capacitor soldered across ESC power (+ / -), insulated, and secured

---

## D) Motors + motor wiring

- [ ] Motors mounted to arms
  - [ ] Correct **M3 screw length** (does not hit windings)
  - [ ] Threadlocker used (if not pre-treated)
  - [ ] No wire pinch points
- [ ] Motor wires routed cleanly along arms
- [ ] Motor wires soldered to ESC pads (M1–M4)
  - [ ] No loose strands / cold joints
  - [ ] Heatshrink/tape/zip ties applied for cable management

---

## E) FC ↔ ESC connection (stack harness)

- [ ] FC–ESC interconnect cable installed (correct orientation)
- [ ] **Pinout confirmed** with your FC/ESC wiring diagrams (not universal)
- [ ] Stack hardware tightened evenly (not crushing soft grommets)

---

## F) Plan UARTs (do BEFORE soldering peripherals)

- [ ] UART map written down (example):
  - [ ] UART __ : Receiver (CRSF/ELRS)
  - [ ] UART __ : VTX control / MSP (digital) / SmartAudio (analog)
  - [ ] UART __ : GPS
  - [ ] UART __ : Spare/accessories
- [ ] FC pads tinned for the peripherals you’ll install

---

## G) Receiver (CRSF / ELRS)

- [ ] Receiver soldered:
  - [ ] GND → GND
  - [ ] VCC → 5V/4V5/9V (as required)
  - [ ] RX (FC) ← TX (RX)
  - [ ] TX (FC) → RX (RX)
- [ ] Antenna mounted with strain relief and safe routing

---

## H) FPV system

### Digital (Walksnail/DJI/HDZero/OpenIPC)
- [ ] Power/GND wired to correct voltage pad
- [ ] UART/MSP/control wiring connected as required
- [ ] Antenna installed and secured (never power VTX without antenna)

### Analog (camera + VTX via FC OSD)
- [ ] Camera → FC:
  - [ ] Video → CAM/VI
  - [ ] Power → regulated 5V/9V (as needed)
  - [ ] GND → GND
- [ ] FC → VTX:
  - [ ] VOUT/VO → VTX Video In
  - [ ] Power → correct VTX voltage pad
  - [ ] GND → GND
  - [ ] SmartAudio/Tramp → chosen UART TX (or dedicated VTX pad)

---

## I) GPS (optional)

- [ ] GPS wired:
  - [ ] GND → GND
  - [ ] VCC → 5V (as required)
  - [ ] TX (GPS) → RX (FC UART)
  - [ ] RX (GPS) → TX (FC UART)
- [ ] Compass (if present): SDA/SCL wired and planned for config

---

## J) Smoke check + electrical validation

- [ ] Visual inspection (bridges, stray strands, polarity, pinched wires)
- [ ] Multimeter check at XT leads (no dead short)
- [ ] First power-up with smoke stopper (recommended)
- [ ] Confirm no abnormal heat/smell and LEDs behave normally

---

## K) Betaflight (props OFF)

- [ ] Connect to Betaflight Configurator
- [ ] Flash correct target (if needed)
- [ ] Ports tab: enable UARTs per your map (Receiver / GPS / VTX/MSP)
- [ ] Receiver tab: confirm channel mapping and correct stick directions
- [ ] Motors tab (props OFF): confirm motor order + direction
- [ ] Configure VTX (tables / MSP / SmartAudio) as needed
- [ ] Failsafe configured and tested (bench test)
- [ ] Save and backup config (CLI diff / dump)

---

## L) Final assembly + pre-flight

- [ ] Wires secured, nothing in prop arc
- [ ] Top plate installed
- [ ] Battery strap(s) routed cleanly (no rubbing on carbon)
- [ ] Props installed correctly (direction + tight)
- [ ] VTX antenna installed before powering
- [ ] Range check / link quality check
- [ ] First hover test in safe area

---

## M) Optional accessories

- [ ] LEDs (addressable): wired to LED pad and configured in Betaflight LED tab
- [ ] Buzzer: wired to buzzer pads and configured
- [ ] Camera switching / MIPI switcher:
  - [ ] Control method chosen (PINIO / cam switch / supported output)
  - [ ] Wiring documented + mapped to a radio switch
