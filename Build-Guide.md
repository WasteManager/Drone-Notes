 General FPV Drone Build Guide (Frame → Wiring → Betaflight)

> This is a **general** assembly flow for a modern FPV quad (5"–7" typical), written so you can reuse it for different frames, FC/ESC stacks, and FPV systems (Analog / Walksnail / DJI / HDZero / OpenIPC).  
> Where relevant, I’ve baked in build considerations pulled from Oscar Liang’s guides (linked in **References**).

---

## 0) Prep: workspace + tools + “don’t skip this” safety

### Workspace
- Good light, ventilation (solder fumes), and a stable bench.
- Magnetic parts tray + labeled bins (M2 vs M3, different screw lengths).

### Tools / supplies (minimum viable)
- Temperature-controlled soldering iron + flux + solder
- Hex drivers (typically 1.5mm / 2.0mm / 2.5mm)
- Flush cutters / wire strippers
- Heatshrink + electrical/cloth tape
- Zip ties + tweezers
- **Multimeter** and/or **smoke stopper / short saver** (highly recommended)

**Why**: A smoke stopper helps limit current on first power-up, but you should still check for shorts with a multimeter first (see smoke-check section).  
Refs: Oscar Liang multimeter guide + smoke stopper notes. citeturn0search1turn0search5

---

## 1) Organize your parts list (and verify compatibility)

### Core parts
- Frame (size, stack mount pattern, camera mount width)
- Motors (KV matched to battery voltage)
- ESC (4-in-1) + Flight Controller (FC) stack
- Props
- Receiver (RX): **CRSF** or **ELRS**
- FPV system:
  - **Analog**: camera + VTX + antenna
  - **Digital**: air unit (Walksnail/DJI/HDZero/etc) + antenna(s)
- Optional: GPS + compass, buzzer, LED strip, camera switcher, etc.

### Compatibility checks (do these before soldering)
- **Stack mount**: 20x20 or 30x30 (and the frame supports it)
- **Voltage**: ESC and VTX support your battery (4S/6S/etc)
- **Props vs motor**: shaft size, prop mounting style
- **Receiver**: FC has a free UART and appropriate voltage pad
- **FPV**: FC has pads for camera/VTX (analog) or a free UART for MSP/OSD (digital)

> Pro tip: do a “dry fit” of components in the frame to confirm placement and wire lengths before you cut anything. citeturn1view0turn4view0

---

## 2) Assemble the frame (leave the top open)

1. Deburr/inspect carbon plates and arms.
2. Assemble bottom plate + arms + standoffs.
3. **Leave the top plate off** so you can reach the stack and route wires.

### Frame-specific considerations (worth doing once, up front)
- **Sand/chamfer sharp carbon edges** (especially arm edges and strap slots) to reduce wire/strap damage.
- **Wash carbon fiber parts** (soapy water), rinse and dry thoroughly to remove carbon dust (carbon dust is conductive).  
Refs: Oscar Liang analog build guide frame prep section. citeturn4view2

---

## 3) Install ESC (dry fit, then commit)

### Orientation + clearance
- Place the **4-in-1 ESC** in the stack location.
- Confirm **nothing can touch**:  
  - ESC underside to carbon frame  
  - ESC to FC (when stacked)  
Refs: Oscar Liang ESC install checks. citeturn4view0turn4view2

### Hardware / vibration philosophy
- Use appropriate standoffs and *consistent tension* on the stack hardware.
- Some builds prefer robust metal stack hardware over fragile nylon, to improve durability and reduce wobble. citeturn4view2

---

## 4) Tin ESC pads

- Tin motor pads (M1–M4) and main power pads (+ / -).
- Aim for a clean solder “dome” on pads before attaching wires. citeturn4view0turn4view2

---

## 5) Solder battery leads + XT60/XT90 pigtail + capacitor

### Decisions to make before soldering
- **XT plug placement**: rear vs side vs top-mount; strain relief plan; battery strap path.
- **Wire gauge (XT pigtail)**: choose based on power draw and frame size.  
Oscar Liang has practical AWG recommendations for FPV builds and XT60 pigtails. citeturn0search2

### Capacitor guidance
- Add a low-ESR capacitor close to ESC power input to reduce voltage spikes/noise and protect electronics. citeturn0search6
- Observe polarity, heatshrink legs, and provide strain relief (don’t let the cap “flop” in crashes).

### Soldering steps
1. Cut pigtail to length (leave enough for serviceability and strain relief).
2. Tin pigtail ends.
3. Solder **+** then **-** to ESC power pads.
4. Solder capacitor across +/-, as close as practical to ESC input (and secure it).

---

## 6) Attach motors to arms (screw length matters)

### Motor screws: do this like you mean it
- Most 22xx–24xx motors use **M3** screws.
- A common rule of thumb: **screw thread length ≈ arm thickness + ~2mm** (verify against your motor base depth).  
Refs: Oscar Liang screw length guidance. citeturn0search3turn0search7
- **Check** that screws do **not** press into motor windings (this can kill motors/ESC). citeturn4view0
- Apply blue threadlocker unless screws are pre-treated. citeturn4view0turn4view2

### Wire protection note
- Route motor wires so they don’t get pinched by arm/skids/guards.
- Avoid crushing motor wires under hardware (too-long screws or poor routing).

---

## 7) Route + solder motor wires to ESC pads (don’t overthink order)

### Order / direction sanity
- Any of the 3 motor wires can go to any of the 3 pads for that motor.
- The order only affects initial spin direction; you can fix direction later in software (or swap any two wires).  
(Practical build principle; you’ll validate in Betaflight motor test.)

### Routing styles (choose your vibe)
Oscar Liang calls out common motor-wire routing approaches (quick & casual vs neat routing vs tight routing). citeturn4view0turn4view2

### Cable management
- Secure motor wires to arms with cloth tape/electrical tape or guides.
- Leave a *little* slack for crashes and maintenance. citeturn4view0turn4view2

---

## 8) Connect FC ↔ ESC (stack interconnect cable)

### The “technical term” you’re looking for
- Common names: **FC–ESC interconnect cable**, **stack cable**, **ESC signal harness**, **8-pin harness**.
- Common connector families in FPV stacks include small JST-style board connectors (exact pitch/type varies by manufacturer).

### Critical caveat
**FC/ESC plugs are NOT universal.** Even when it “fits,” pinouts can differ.
- Use the **wiring diagram** for *your specific* FC and ESC.
- Confirm:
  - VBAT / GND (if present)
  - Current sensor, voltage sense
  - Motor signals (S1–S4) and ground reference
  - Telemetry (ESC-TLM), etc.

> If your ESC is rotated (e.g., to fit frame lead routing), plan for motor remapping later in Betaflight. citeturn4view0

---

## 9) Tin FC pads (and plan your UARTs before you solder)

### Tin FC pads
- Tin the pads you’ll use for RX, VTX, GPS, LEDs/buzzer, etc.

### UART planning (high leverage)
Make a deliberate UART map **before** soldering:
- UART for **Receiver** (CRSF/ELRS)
- UART for **VTX control** (SmartAudio/Tramp) or **digital MSP/OSD**
- UART for **GPS**
- Spare UART(s) for accessories (rangefinder, telemetry, camera control, etc.)

**Document it in the build notes** so Betaflight setup is painless later.

---

## 10) Install receiver (CRSF or ELRS)

### Wiring rules of thumb
- **GND → GND**
- **5V/9V/4V5 → VCC** (use the voltage your RX expects)
- **RX pad on FC ← TX from receiver**
- **TX pad on FC → RX on receiver**  
(“RX to TX, TX to RX”)

### Mechanical considerations
- Plan antenna placement and coax routing so:
  - Antenna elements are away from carbon and high-current wires
  - You have strain relief
  - It won’t get chopped by props in a crash

---

## 11) Install FPV system

## 11A) Digital VTX / air unit (Walksnail / DJI / HDZero / OpenIPC)
General wiring pattern (varies by system):
- Power and ground to appropriate pads (observe voltage requirements)
- One UART for MSP/OSD/control (common)
- Some systems also use additional pads for camera control or other features (read your manual)

**Best practice:** follow the manufacturer wiring diagram *and* your FC’s pad labels. The pin labels differ between FCs.

## 11B) Analog camera + analog VTX (with FC OSD)
Analog typically looks like this:

**Camera → FC**
- Camera **Video** → FC **CAM** (or “VI”/“CAM IN”) pad
- Camera **GND** → FC **GND**
- Camera **5V/9V** → FC regulated camera power pad

**FC → VTX**
- FC **VTX** (or “VO”/“VOUT”) → VTX **Video In**
- VTX **GND** → FC **GND**
- VTX **Power** → correct VTX voltage pad (often 9V/10V, sometimes VBAT)
- VTX control:
  - **SmartAudio/Tramp** wire → a UART TX pad (or dedicated VTX control pad if your FC has one)

> If you bypass the FC for video (camera straight to VTX), you won’t get Betaflight OSD overlay.

---

## 12) Install GPS (and optional compass)

Typical GPS module wiring:
- **GND → GND**
- **VCC → 5V** (or whatever the GPS specifies)
- **GPS TX → FC RX (on chosen UART)**
- **GPS RX → FC TX (on chosen UART)**

If your GPS has a compass/magnetometer:
- **SDA → SDA**
- **SCL → SCL**
…and ensure Betaflight supports/configured for it.

---

## 13) Smoke check + multimeter checks (before full power)

### 13A) Visual inspection (fast but mandatory)
- No solder bridges
- No loose wire strands
- Capacitor polarity correct
- No pinched wires under stack
- No exposed conductors that can touch carbon

### 13B) Multimeter resistance check at the battery leads
- With the drone **unpowered**, measure resistance across the XT pigtail (+ to -).
- If you see **near 0 ohms**, treat it as a short and fix it *before* applying power. citeturn0search5  
(Exact readings vary because capacitors charge/discharge; the key is not being “dead short.”)

### 13C) First power-up using a smoke stopper / short saver
- Insert smoke stopper between battery and quad.
- Plug in battery and watch for:
  - Smoke stopper tripping immediately
  - Heat/smell
  - LEDs behaving normally  
Refs: Oscar Liang smoke stopper + multimeter guidance. citeturn0search1turn0search5

---

## 14) Plug into Betaflight and do initial configuration

High-level flow (follow Betaflight docs for details):
1. Flash correct firmware target (if needed).
2. Calibrate accelerometer (if you use it).
3. Set **Ports** (UART mapping):
   - Receiver UART
   - GPS UART
   - VTX control UART / MSP displayport (digital)
4. Receiver tab: confirm inputs are correct and centered.
5. Setup motor direction check (props OFF):
   - Confirm motors mapped correctly (especially if ESC rotated).
   - Confirm motor direction (fix by software or swapping wires).
6. OSD / VTX tables (as needed).
7. Failsafe settings.
8. Save/backup your config.

---

## 15) Final assembly: top plate, cleanup, and pre-flight checklist

### Mechanical close-out
- Secure all wires (zip ties, tape, heatshrink, printed guides)
- Install top plate
- Ensure:
  - No wires are in prop arc
  - Antennas are secured and not stressed
  - Battery strap path is clean and won’t cut wires

### Pre-flight checklist (first flights)
- Props installed correct direction
- Motor screws tight (threadlock)
- VTX antenna installed before powering VTX
- Receiver link stable + correct failsafe
- GPS lock (if used)
- Arm/disarm tested safely

---

## 16) Accessories and “extra credit” considerations

### LEDs
- Addressable LED strips usually go to a dedicated **LED** pad (not a UART) and use Betaflight LED Strip configuration.
- Simple “lights” that are just power can be driven from 5V/GND (with a switch if desired).

### Buzzer
- Use dedicated buzzer pads if present; configure in Betaflight.

### Multi-camera / camera switching (MIPI / analog switchers)
- **MIPI switchers** (for digital camera routing) typically need:
  - Power + ground
  - A control interface (often GPIO-style control lines, sometimes a serial interface depending on the switcher)
- In Betaflight, camera switching and accessory control is commonly done using:
  - A dedicated **camera control / cam switch** feature *if your FC supports it*, or
  - **PINIO** (FC GPIO output) mapped to a switch on your radio, or
  - Other supported outputs (depending on your FC target and available pads)

**Build guidance:**
- Pick the control method early (so you reserve the right pad/UART/GPIO).
- Route the switcher wiring cleanly and keep it away from high-current battery leads where possible.
- Document which control line maps to which Betaflight function.

---

## References (Oscar Liang)
These are the main sources used for the considerations embedded above:
- How to build an FPV drone (general build flow + ESC/motor wiring + dry-fitting) citeturn1view0turn4view0
- Analog build guide (frame prep details + stack hardware/vibration notes + wiring discipline) citeturn4view2
- Wires & connectors / AWG guidance citeturn0search2
- Screw selection and motor screw length rules of thumb citeturn0search3turn0search7
- Why capacitors matter on FPV drones citeturn0search6
- Smoke stopper + multimeter testing guidance citeturn0search1turn0search5
