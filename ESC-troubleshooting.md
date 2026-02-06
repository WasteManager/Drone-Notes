> **Scope:** Continuity-based troubleshooting + a practical “pilot-friendly” method.  
> **Not scope:** Deep MOSFET theory, oscilloscope-level analysis, firmware issues.

---

## What you need

- Multimeter with **continuity mode** (beep mode)
- Probes/leads
- (Strongly recommended) **Bench power supply** with **current limit** and adjustable voltage  
  - Using a LiPo for first power-up is how you turn “maybe fixable” into “definitely cooked.”

---

## Safety rules (do not skip)

1. **No battery connected** during continuity tests.
2. **Remove motors** (or at least motor wires) when checking ESC phase pads for shorts:
   - Motors can create **false readings** because phases are connected through windings.
3. When using a bench PSU:
   - Start with **low voltage** and **tight current limit**
   - If it gets **hot fast** or the PSU alarms, **stop** immediately.

---

## Multimeter setup (continuity)

- Set meter to **continuity** (beep symbol)
- Confirm it works by touching probes together → **beep**

---

## ESC Triage Workflow (fast → deeper)

### Step 1 — Check for a dead short across VBAT (main power pads)

**Goal:** Is there a short between **BAT+** and **BAT− (GND)**?

1. Probe **BAT−** with one lead.
2. Probe **BAT+** with the other lead.

**Result interpretation**
- ✅ **No continuous beep**: you likely do **not** have a hard short across the main rails.
- ❌ **Continuous beep**: hard short → do not power the ESC; inspect for damage/bridges, failed components.

> Note: Depending on capacitor behavior, some setups may show brief activity. You’re looking for **continuous** continuity.

---

### Step 2 — Check motor phase pads for shorts (phase-to-phase), per motor group

**Goal:** Detect internal shorts between phases for a given motor output.

**Important:** **No motor connected** (otherwise false readings).

Each motor output has **three pads** (A/B/C). Test each group:

- Within a motor group:
  - Phase1 ↔ Phase2
  - Phase2 ↔ Phase3
  - Phase1 ↔ Phase3

**What you expect**
- In the transcript test case, there was **no continuity** phase-to-phase (no beep).  
- If you *do* get continuity here (with no motor attached), treat it as a red flag and proceed to MOSFET checks.

> Tip: Stay **within one motor’s 3 pads**. Don’t mix across motors for this step.

---

### Step 3 — Check BAT− and BAT+ to motor phase pads (the “pilot-friendly MOSFET clue”)

This is the test that found the real issue in the transcript.

#### 3A) BAT− (GND) → each motor phase pad
1. Put one probe on **BAT−**
2. Touch the other probe to each **motor phase pad**, for all motors

**Expected**
- Typically **no continuity** to phases in this simple screening test (no beep).

#### 3B) BAT+ → each motor phase pad
1. Put one probe on **BAT+**
2. Touch the other probe to each **motor phase pad**, for all motors

**Interpretation (per transcript)**
- ✅ If you **do not** get continuity: good sign
- ❌ If you get **continuity between BAT+ and a motor phase pad**: strong indicator that a **MOSFET is failed short** (or a related power-stage fault) on that motor channel

In the transcript case:
- Multiple motor channels showed **BAT+ → phase continuity**, pointing to **multiple bad MOSFETs**.

---

## Confirming the diagnosis with a bench power supply (recommended)

Even if continuity checks look “mostly fine,” a PSU can reveal a fault immediately.

### Safe power-up procedure
1. Set PSU voltage to a low value (example in the transcript: ~11.5V for a 3S-ish test).
2. Set a **low current limit** (start conservative).
3. Connect **GND**, then **BAT+**.

**Warning signs**
- PSU alarms/screams (overcurrent)
- ESC becomes **toasty super fast**
- Unexpectedly high current draw at idle

These are classic symptoms of a shorted power stage (often MOSFET-related).

---

## MOSFET testing (continuity-based, practical version)

If you’ve identified “which motor channel is weird” via BAT+→phase continuity, you can go further and test the MOSFETs on that channel.

### What you need to know (minimum)
- MOSFET pins: **Gate (G)**, **Drain (D)**, **Source (S)**
- The **Gate** controls whether current can flow between **Drain** and **Source**
- If your meter shows continuity where it should not (D↔S conducting “all the time”), that MOSFET is likely **bad (shorted)**

### How the transcript tests them
- Identify the MOSFET part number (e.g., marked on the package) and look up its pinout/datasheet.
- Use continuity to “map” which legs/pads are connected.
- Check for **unexpected continuity** across the wrong legs (indicating a shorted device).

> If you’re not comfortable at this step, the earlier tests (BAT+→phase continuity) are often enough to decide “replace ESC” vs “attempt repair.”

---

## Practical repair notes (if you’re replacing MOSFETs)

The transcript demonstrates a successful repair by replacing **three MOSFETs** on a high-end ESC.

### If you attempt MOSFET replacement
- **Ventilation matters**: burning flux/solder fumes are no joke—use a fan, fume extractor, or purifier.
- **Mark your bad MOSFETs** so you don’t get confused:
  - Scratch mark with probe tip, or use a sharpie/pencil/crayon.
- Protect connectors from heat:
  - Use **Kapton tape** or **copper tape**
- Warm the board evenly; keep the hot air moving (avoid cooking one spot).
- Clean pads with solder wick; re-tin lightly; use flux.
- Re-check for bridges before power.

> Rework is advanced. If you’re not set up for hot air + rework, this is your sign to replace the ESC.

---

## Post-repair validation checklist (must do)

### 1) Repeat continuity tests
- [ ] BAT+ ↔ BAT−: no continuous beep
- [ ] BAT− → all motor phase pads: no unexpected continuity
- [ ] BAT+ → all motor phase pads: **no continuity** (this was the key failure mode)

### 2) Bench PSU test
- [ ] Power ESC at safe voltage with current limit
- [ ] No alarms
- [ ] No rapid heating
- [ ] Low idle current draw (transcript example: ~11 mA after repair)

### 3) Only then install into a quad
- [ ] Full functional test in the airframe
- [ ] Motor spin test (props off)
- [ ] Smoke stopper for first LiPo plug-in (optional but smart)

---

## Quick decision guide

### Replace the ESC if:
- VBAT short is present
- BAT+ → phase continuity persists across multiple pads
- PSU overcurrent + rapid heating
- You don’t have rework tools/experience

### Consider MOSFET repair if:
- It’s a high-value ESC
- You can identify the failed MOSFET(s)
- You have hot air/rework setup and can source replacement parts

---

## TL;DR checklist (copy/paste)

- [ ] Props off / no battery
- [ ] Continuity mode confirmed
- [ ] BAT+ ↔ BAT− (no continuous beep)
- [ ] **No motors connected** for phase testing
- [ ] Phase-to-phase within each motor group (check for red flags)
- [ ] BAT− → each phase pad (no beep expected)
- [ ] **BAT+ → each phase pad** (beep here strongly suggests bad MOSFET on that channel)
- [ ] Bench PSU test with current limit (no alarm, no rapid heat)

