# FPV RF Study Sheet (Control + Video)

Control links (TBS Crossfire vs ExpressLRS) and video links (Analog vs Walksnail), from fundamentals to practical tuning.

---

## How to use this sheet
Start at **RF fundamentals** for intuition, then work down into **link budget**, **frequency/band choices**, **antennas/polarization**, and **system-specific tuning**.  
Use it as a decision checklist: **band selection → antenna choice → power → placement → channel planning**.

---

## Contents
1. [RF basics and what actually matters](#1-rf-basics-and-what-actually-matters)  
2. [Link budget in plain English](#2-link-budget-in-plain-english)  
3. [Frequency bands and propagation](#3-frequency-bands-and-propagation)  
4. [Crossfire vs ExpressLRS (ELRS)](#4-crossfire-vs-expresslrs-elrs)  
5. [Output power, watts, and what “more mW” really buys you](#5-output-power-watts-and-what-more-mw-really-buys-you)  
6. [Antennas and polarization: LHCP vs RHCP, omni vs Yagi](#6-antennas-and-polarization-lhcp-vs-rhcp-omni-vs-yagi)  
7. [Video link: Analog vs Walksnail through an RF lens](#7-video-link-analog-vs-walksnail-through-an-rf-lens)  
8. [Practical setup and troubleshooting checklist](#8-practical-setup-and-troubleshooting-checklist)

---

## 1. RF basics and what actually matters

- **RF link = signal + noise + timing.** You need enough signal at the receiver, above the noise floor, with enough margin to survive fades and multipath.
- **Range and penetration are not just “power.”** Frequency band, antenna gain/polarization, receiver sensitivity, packet rate, and environment often dominate.
- **Multipath + Fresnel zone:** flying near the ground or reflective objects (cars, buildings, water) creates destructive interference. Antenna choice/placement and polarization help.
- **Legal + thermal constraints:** higher TX power increases heat and current draw and may exceed local limits. Better antennas/placement often beat “cranking watts.”

### Key vocabulary
- **dB:** ratio. Add/subtract in dB to do RF math quickly.
- **dBm:** power referenced to 1 mW. `0 dBm = 1 mW`. `+10 dB = 10× power`.
- **Noise floor:** baseline RF noise level (receiver + environment).
- **SNR:** signal-to-noise ratio. Higher is better.
- **RSSI / LQ:** received signal / link quality metrics (implementation-specific).
- **Fade margin:** extra headroom so the link survives momentary drops.

---

## 2. Link budget in plain English

A link budget is just:

> **how strong the signal arrives** − **how hard the receiver can hear** − **how noisy the world is**

| Concept | What it means for FPV |
|---|---|
| TX power (dBm) | More power helps, but with diminishing returns and more heat. `+3 dB` is only `~2× power`. |
| TX antenna gain (dBi) | Directional antennas focus energy. Higher gain usually means a narrower beam. |
| Path loss | Loss over distance + losses through walls/trees/body absorption. Higher frequency loses more for the same path. |
| RX antenna gain (dBi) | Better antennas and diversity (two different antennas) can stabilize video and control. |
| Receiver sensitivity | How weak a signal the receiver can still decode at a target bitrate/packet rate. |
| Noise floor | Wi‑Fi, other pilots, electronics, and the receiver itself raise noise and reduce usable range. |

### Rules of thumb (the dB math you actually use)
- **+3 dB ≈ 2× power**, **+6 dB ≈ 4×**, **+10 dB = 10×**
- Free-space path loss increases roughly with **20·log10(distance)** and **20·log10(frequency)**
- If your link is “almost” good, a **3–6 dB** improvement (antenna, placement, polarization, cleaner channel) can flip you from “failsafe-y” to solid

---

## 3. Frequency bands and propagation

Lower frequency generally penetrates/diffracts better, but often means larger antennas and less bandwidth.  
Higher frequency supports higher data rates (great for digital video) but becomes more line-of-sight sensitive.

| Band | Typical FPV use | Strengths | Tradeoffs |
|---|---|---|---|
| ~868/915 MHz | Long-range control (Crossfire, ELRS sub‑G) | Better penetration; better around foliage/terrain; more forgiving of partial obstructions | Larger antennas; less spectrum for high‑rate data; can be crowded |
| 2.4 GHz | Control (ELRS 2.4) | Small antennas; high packet rates; strong ecosystem | More obstacle attenuation than sub‑G; more interference (Wi‑Fi/Bluetooth); lively multipath |
| 5.8 GHz | Video (Analog, Walksnail) | Lots of bandwidth; small antennas; many channels | Worst penetration; highly LoS; sensitive to body blocking and Fresnel clearance |

### Penetration vs range: a practical model
- **Line of sight is king at 5.8 GHz.** Even thin obstacles and your body can cause big fades.
- **Sub‑G control is more forgiving** behind a hill/treeline, but not magic—terrain can still fully shadow you.
- **Frequency choice is a system decision.** Most pilots run control on sub‑G or 2.4, and video on 5.8 because video needs bandwidth.

---

## 4. Crossfire vs ExpressLRS (ELRS)

> **Note:** CRSF is also the serial protocol between receiver and flight controller. In FPV slang, people often say “CRSF” to mean **TBS Crossfire** (the control ecosystem). This compares **TBS Crossfire** vs **ExpressLRS**.

| Category | TBS Crossfire (usually 868/915) | ExpressLRS (2.4 and 868/915) |
|---|---|---|
| Primary design goal | Reliable long‑range; mature ecosystem | Open source; high performance; flexible configs |
| Bands | 868 or 915 MHz (region‑dependent) | 2.4 GHz and sub‑G (868/915) depending on hardware |
| Range / penetration | Excellent with good antennas/placement, especially sub‑G | Excellent on sub‑G; 2.4 can be strong but more obstacle‑sensitive than sub‑G |
| Latency | Very good; depends on mode/telemetry load | Can be extremely low latency at high packet rates (trade margin for speed) |
| Telemetry | Robust and integrated; may adapt rate | Highly configurable; trade packet rate vs telemetry vs RF conditions |
| Ecosystem / UX | Polished hardware + support; often “it just works” | Community‑driven; fast‑moving; more knobs/options |
| Cost | Generally higher | Often lower; wide vendor selection |

### Practical takeaways
- If you fly **behind terrain/trees** or want maximum forgiveness, **sub‑G** control is usually the safer default (Crossfire or ELRS sub‑G).
- If you want **very low latency** and high refresh, **ELRS 2.4** is common—respect interference and antenna placement.
- Both can fail with bad antenna placement/shadowing. **Mechanical setup matters as much as the protocol.**

---

## 5. Output power, watts, and what “more mW” really buys you

RF output is often shown in **mW** or **dBm**. RF math is logarithmic → increasing watts gives diminishing returns.

| mW | dBm (approx) | Notes |
|---:|---:|---|
| 25 | 14.0 |  |
| 50 | 17.0 |  |
| 100 | 20.0 | Common baseline |
| 250 | 24.0 |  |
| 500 | 27.0 |  |
| 1000 | 30.0 | 1 W: heat + battery draw jump |
| 2000 | 33.0 | Often impractical on small gear |

### ERP/EIRP: the power that actually leaves the antenna
- **TX power** is what the radio outputs.
- **ERP/EIRP** is what you effectively radiate after **antenna gain** and **cable losses**.
- A better antenna (or less coax loss) can beat a power increase while running cooler.

Rules:
- Doubling TX power (e.g., **250 → 500 mW**) is only **+3 dB**
- A modest antenna upgrade (**+2 to +3 dBi**) can be equivalent to doubling power
- Cable/connector losses matter at **5.8 GHz**: keep coax short, avoid sharp bends, use quality connectors

---

## 6. Antennas and polarization: LHCP vs RHCP, omni vs Yagi

Antennas are the highest leverage part of your RF chain. Treat them as flight-critical components.

### Polarization in FPV
- **Linear** (straight element antennas): simple/cheap; more vulnerable to multipath near reflective surfaces.
- **Circular** (LHCP/RHCP): common on 5.8 video; reduces multipath and stays usable through reflections.
- **Match matters:** LHCP↔LHCP and RHCP↔RHCP. Mismatch introduces significant loss.
- **Control links** are often linear on 868/915 and 2.4, but circular can be used in some setups.

### Omni-directional vs directional
| Type | Examples | When to use | Gotchas |
|---|---|---|---|
| Omni | Dipole, Cloverleaf, Pagoda | General flying; freestyle; close to mid range | Lower gain; still has nulls depending on design |
| Directional | Patch, Helical, Yagi | Long‑range; fixed direction; base station aiming | High gain but narrow beam; off-axis loss is steep |

### Practical antenna placement rules
- Keep control and video antennas away from **carbon fiber**, **battery**, and **high-current wiring** (detuning + shadowing).
- Give antennas **line of sight** (rear-mount behind battery often blocks on punch‑outs).
- **Diversity helps:** omni + patch at goggles/VRX covers weak spots.
- Maintain polarization orientation: linear needs alignment; circular needs LHCP/RHCP matching.

---

## 7. Video link: Analog vs Walksnail through an RF lens

Both commonly run on **5.8 GHz**, but behave differently due to encoding/error correction.

| Topic | Analog 5.8 | Walksnail (digital) 5.8 |
|---|---|---|
| Breakup behavior | Gradual snow/lines; often still flyable | More cliff-like: clean → pixelate/drop when SNR falls |
| Interference tolerance | Can tolerate weak signals but shows noise | Error correction keeps image clean until it can’t |
| Latency | Very low | Low but typically higher depending on mode/settings |
| Channel planning | Frequency separation helps; adjacent bleed is real | Also needs planning; digital uses wider bandwidth |
| Range/penetration feel | Often “limps” farther with noise | Strongly SNR-dependent; good antennas + clean channels are key |

### Why 5.8 GHz video feels fragile
- Short wavelength means your body/quad/small objects cause strong shadowing.
- Fresnel clearance matters: flying low behind brush/dips can kill 5.8 even with partial LoS.
- Multipath is severe in urban areas → circular polarization + diversity is standard mitigation.

### Analog + digital coexistence notes
- Every VTX can **desense** nearby receivers: separate video TX and control RX antennas on the quad.
- At the ground station, keep high-power video transmitters away from sensitive control receivers.
- Group flying requires **channel discipline**; avoid adjacent channels where possible.

---

## 8. Practical setup and troubleshooting checklist

### Pre-flight RF sanity check
- **Control link:** correct band/region; antennas secured and not pinched; verify link quality + telemetry at arm distance.
- **Video:** match polarization (LHCP↔LHCP / RHCP↔RHCP); confirm channel and power; confirm VRX antennas are intentional (omni vs patch).
- **Placement:** keep antennas away from carbon plates, battery, high-current wires; don’t bury antennas inside the frame.
- **Ground station:** directional RX for long range; keep it aimed; pair with an omni if you move around.
- **Thermals:** high VTX power without airflow overheats quickly (bench testing kills VTXs).
- **Interference:** sudden performance change → suspect nearby Wi‑Fi, other pilots, damaged coax/connectors, or swapped polarization.

### Quick diagnostic cues
| Symptom | Likely cause | Fastest test/fix |
|---|---|---|
| Video is great then suddenly drops | SNR cliff (digital), antenna aim, or obstruction | Use higher-gain directional RX; re-aim; gain altitude; check Fresnel clearance |
| Video always noisy at close range | Wrong channel/polarization, damaged antenna, VTX power stuck low | Confirm freq/polarization; swap known-good antenna; verify VTX power |
| Control LQ drops on punch‑outs | Body/battery shadowing, antenna nulls, RX near noise source | Move RX antennas to clear air; change orientation; separate from VTX + power wiring |
| Works solo, fails in group | Channel overlap / desense | Spread channels; reduce power; separate pilots; verify band planning |

### If you remember only 5 things
1. Antenna placement and polarization often beat raw TX power.
2. Sub‑G control penetrates better than 2.4, and both beat 5.8 for obstruction handling.
3. Digital video looks perfect until it doesn’t—protect SNR with good antennas and clean channels.
4. Directional antennas are a force multiplier for long range, but you must aim them.
5. Group flying needs channel planning; adjacent channel overlap can ruin everyone.
