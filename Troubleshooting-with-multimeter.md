## Why you want a multimeter (FPV context)

A cheap multimeter with continuity mode can help you:

- **Verify you *don’t* have a short** before plugging in a LiPo (prevents “magic smoke”).
- Find **why motors get hot** (electrical short or mechanical screw-to-winding contact).
- Identify **solder bridges** between adjacent pads that are hard to see.
- Confirm whether a “beep” is **normal** (expected connections) or **bad** (unexpected short).

---

## What you need

- A multimeter with **continuity mode** (often shown as a symbol like a **Wi‑Fi / sound wave** icon).
- Two probes/leads:
  - **Black lead**
  - **Red lead**

---

## Safety rules (read first)

1. **Remove the LiPo** before continuity testing your quad.
2. **Never continuity test while powered** (battery connected).
3. If you’re unsure, continuity test **before first plug‑in** after any soldering or rebuild.

---

## Multimeter setup (continuity mode)

### 1) Plug in the leads correctly
- **Black lead → COM**
- **Red lead → V / Ω / mA (the port labeled with V, ohms, and typically a squiggly line)**

> Do **not** put the red lead into the high‑current port unless you’re explicitly measuring current (not covered here).

### 2) Select continuity mode
Turn the dial to the **continuity** symbol (often looks like a **Wi‑Fi** icon or sound waves).

### 3) Confirm it works
Touch the two probe tips together:
- If it **beeps**, continuity mode is working.

---

## Understanding beeps: “good beeps” vs “bad beeps”

### Good beep (expected)
A beep is “good” when you probe **two points that are supposed to be connected** electrically.

Examples (common on flight controllers):
- **5V pads to 5V pads** (they’re usually all tied together on the board)
- **9V pads to 9V pads** (same idea)

### Bad beep (unexpected)
A beep is “bad” when you probe **two points that should NOT be connected**, especially:
- **Two adjacent pads** that are not the same rail (not both 5V, not both 9V)
- Random signal pads beeping together (RX to TX, signal to ground, etc.)
- Motor phase pads from **different motors** beeping together (on the ESC)

### Momentary beep (often normal)
Sometimes you’ll hear a beep briefly and it fades away (or the reading ramps then settles).  
That often happens with **capacitors charging** and can be normal.

**What to worry about:**
- **Continuous beep** = likely short
- **Momentary beep that stops** = often normal (especially across a capacitor)

---

## Workflow: How to check for shorts like a pro

### Step 1 — Quick “is there a short” sanity test (before LiPo)
- Put one probe on **battery negative (GND)** and the other on **battery positive (VBAT)** points (often the ESC battery pads).
- If you get a **solid continuous beep**, you likely have a short across the main power rail.

> Note: some builds may show a brief reaction because capacitors charge; the key is whether it becomes a **constant beep**.

---

## Flight controller checks (most common solder‑bridge problems)

### What you’re looking for
The most common FC short is a **solder bridge** between two nearby pads (sometimes microscopic).

### How to test
1. Keep the quad **unpowered**.
2. In continuity mode, probe:
   - **Adjacent pads** around areas you soldered (UARTs, receiver pads, camera/VTX pads, etc.)
3. If two pads you *didn’t intend* to connect **beep**, inspect closely:
   - Look for tiny solder strands/bridges
   - Reflow with flux and clean the bridge

### “Good beep” example on FC
- Probe **5V pad** to another **5V pad** → **beep is normal**.

---

## ESC checks (motor phase pad continuity)

### Key concept
Each motor has **three phase wires**. Those three wires are connected together through the motor windings.

### What’s normal
- Probing **within the same motor’s three phase pads** may beep (this can be normal depending on what exactly you touch and the motor/ESC design).

### What’s *not* normal (big red flag)
- Probing a motor wire/pad from **Motor A** to a motor wire/pad from **Motor B**:
  - If those **beep together**, it suggests an unintended connection inside the ESC or due to solder/contamination.

### Quick test
- Pick a phase pad from Motor 1.
- Probe a phase pad from Motor 2.
- Repeat across all motor groups.

**Any continuity between different motors’ phases = investigate immediately.**

---

## Motor screw shorts (the “silent killer”)

### What happens
Motor screws can be **too long** and touch the motor windings.  
That can create a short that travels through the **conductive carbon fiber frame**, causing:
- Hot motors
- Hot frame
- System instability or full failure

### Why carbon matters
Carbon fiber is **conductive**. If a screw contacts windings and also touches the frame, the quad can become part of the short path.

### How to test (per motor)
1. Put one probe on a **motor winding / motor wire / motor pad** (any motor phase point is fine).
2. Put the other probe into the **motor screw hole** (or touch the screw head / screw itself).
3. If it **beeps continuously**:
   - That motor likely has a screw contacting windings.
   - Back out the screws and try shorter screws or add washers.

Repeat for **all four motors**.

---

## Capacitors: the “beep that fades” (usually normal)

### What you’ll see
When probing across a capacitor:
- The reading may **ramp**, and some meters may **beep briefly** and then stop.

### Interpretation
- **Momentary beep then stops** → normal capacitor charging behavior.
- **Constant continuous beep** → potential shorted capacitor or downstream short.

### Quick reset trick (discharge)
If the capacitor seems “stuck”:
- Briefly short the capacitor leads **with the probes** to discharge it, then test again.

---

## Troubleshooting cheat sheet

### Symptom → likely cause → what to continuity test

- **Hot motors after build**
  - Likely: solder bridge on FC, ESC issue, motor screw short
  - Test: FC adjacent pads; ESC phase-to-phase across motors; motor phase to screw hole

- **Hot carbon frame**
  - Likely: motor screw short to windings + frame conduction
  - Test: motor phase/wire to screw hole; continuity across frame to confirm conduction

- **Quad smokes on plug-in / instant failure**
  - Likely: main VBAT short or solder bridge on ESC battery pads
  - Test: VBAT+ to GND on ESC; inspect battery pads and capacitor polarity

---

## Best practices (habit to build)

- Continuity test **after every soldering session**.
- Continuity test **before the first LiPo plug‑in** on a new build.
- If you find a short:
  - Assume it’s a **tiny solder bridge** until proven otherwise.
  - Then check **motor screws** (especially if motors are heating).

---

## Quick checklist (copy/paste)

- [ ] LiPo unplugged
- [ ] Black lead in **COM**
- [ ] Red lead in **V/Ω**
- [ ] Dial set to **continuity**
- [ ] Probes touch → meter beeps (mode confirmed)
- [ ] VBAT+ ↔ GND check (watch for continuous beep)
- [ ] FC: check adjacent pads around recent soldering
- [ ] FC: 5V↔5V beeps are normal
- [ ] ESC: phase pads *between different motors* should NOT beep
- [ ] Motor screws: motor phase ↔ screw hole should NOT beep
- [ ] Capacitor: brief beep that fades can be normal; continuous beep is not

---

## Notes
This guide intentionally focuses on continuity mode because it’s the fastest way to catch shorts that destroy FPV builds. If you want, I can add an “advanced” section for:
- Measuring **VBAT voltage**
- Checking **5V regulator output**
- Measuring **resistance to ground**
- Continuity testing **signal wires** end-to-end (receiver, VTX, camera)
