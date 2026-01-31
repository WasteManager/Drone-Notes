# ExpressLRS (ELRS) Flashing & Updating Guide (TX + RX) + Betaflight Nuances

This guide covers:
- Flashing/updating **ExpressLRS transmitter module** (internal or external)
- Flashing/updating **ExpressLRS receiver**
- Key “whys” and pitfalls (versioning, binding phrase, regulatory domain, WiFi updates)
- Betaflight configuration considerations for ELRS (CRSF protocol)

> Big concept: ELRS uses the **CRSF serial protocol** between the receiver and Betaflight FC. In Betaflight you still select **CRSF** as the serial receiver provider.

---

## 1) What ELRS “is” in practical terms (and why it matters)

### Why ELRS is popular
- **Open-source**, rapid development, broad hardware ecosystem.
- Strong performance: high packet rates, low latency, long-range capability (hardware-dependent).
- Great telemetry and link health metrics (**LQ**, **RSSI dBm**, etc.).

### Nuances you must respect
- **Major version compatibility:** ELRS TX and RX must match major versions (e.g., 3.x ↔ 3.x). Minor versions can differ, but major mismatches generally won’t bind.
- **Regulatory domain matters:** you must pick the correct domain for your region (legal + frequency/channel behavior).
- **Binding phrase is high-leverage:** use a binding phrase to avoid “bind button rituals” and make rebuilds painless.

---

## 2) Update strategy (recommended)

### “Why” order matters
- Update **TX module** first so you know what major version you’re targeting.
- Then update the **receiver** to match that major version.
- Confirm bind + telemetry, then do Betaflight configuration.

---

## 3) Tools you’ll use

### ExpressLRS Configurator (primary)
- Used to select targets, set options, build firmware, and flash.

### Receiver update options (most common)
1. **WiFi update** (if your receiver supports it / has WiFi mode)
2. **UART flash** (wired directly to receiver boot pads/UART)
3. **Betaflight Passthrough** (flash receiver via FC connection without extra adapters)

**Why multiple methods exist:** different receivers and situations (no WiFi credentials set, inaccessible receiver, FC already installed, etc.).

---

## 4) Flash / update ELRS transmitter module (TX)

### General workflow
1. Open ExpressLRS Configurator.
2. Select **Device Category** = TX.
3. Select the exact **Device Target** that matches your module.
4. Choose the **Release** you want (stay on official stable unless you need a feature).
5. Set key options:
   - Regulatory domain (mandatory)
   - Binding phrase (optional but recommended)
   - Packet rate defaults (start conservative; adjust later)

6. Flash using the method your TX supports:
   - USB / serial
   - WiFi (some modules)
   - Other module-specific methods

**Nuances**
- Keep a consistent binding phrase across your fleet.
- Don’t chase 1000Hz packet rates on hardware that can’t sustain it; link stability beats bragging rights.

---

## 5) Flash / update ELRS receiver (RX)

### A) WiFi Update (common when the RX has WiFi mode)
**Why you’d choose WiFi:** simplest field update; no disassembly if you can trigger WiFi mode.

General flow:
1. Put receiver in WiFi mode (method varies; often power-cycle 3 times or wait a timeout).
2. Connect to the receiver’s WiFi AP.
3. Upload the firmware file or use the configurator’s WiFi flash workflow (depends on generation).
4. Reboot and verify bind.

### B) Betaflight Passthrough (very common once installed)
**Why you’d choose this:** receiver already installed, and you want a clean update through the FC.

General flow:
1. Connect FC to computer via USB.
2. **Do NOT connect** with Betaflight Configurator while flashing passthrough.
3. In ExpressLRS Configurator:
   - Select your RX target
   - Flash method: **Betaflight Passthrough**
   - Build + Flash
4. When complete, disconnect/reboot and verify link.

### C) UART flash (wired)
**Why you’d choose this:** WiFi not working, passthrough not possible, or first flash on a blank receiver.

General flow:
- Use a USB-UART adapter (or FC passthrough in a different mode, depending on target docs).
- Follow the exact receiver target’s instructions.

---

## 6) Betaflight configuration for ELRS (CRSF protocol)

### 6.1 Wiring expectations
ELRS receivers use CRSF protocol on a full UART pair:
- Receiver **TX → FC RX**
- Receiver **RX → FC TX**
- Power + GND

**Why:** CRSF is bidirectional. Telemetry is part of the link.

### 6.2 Betaflight: Ports tab
- Enable **Serial RX** on the UART you wired to your ELRS receiver.

### 6.3 Betaflight: Configuration tab
- Receiver mode: **Serial (via UART)**
- Serial Receiver Provider: **CRSF**

### 6.4 CLI sanity (this solves a ton of “no stick input” issues)
Ensure:
- `serialrx_inverted = off`
- `serialrx_halfduplex = off`

**Why:** CRSF requires uninverted and full duplex. Old SBUS habits (inversion/half-duplex) break ELRS/CRSF.

### 6.5 Telemetry
- Enable telemetry in Betaflight (recommended).
- On your radio, re-discover sensors after major changes.

---

## 7) ELRS-specific “why” knobs (link quality, packet rate, telemetry ratio)

### Link Quality (LQ) > RSSI
- LQ is a better “are we dropping packets?” indicator than classic RSSI.
- Use LQ thresholds to guide safe limits.

### Packet rate tradeoffs
- Higher packet rate can mean lower latency but reduced range margin (depends on modulation and hardware).
- If you see frequent LQ drops or failsafes, back off packet rate and/or raise telemetry ratio.

### Dynamic power (if supported)
- Dynamic power helps maintain link while conserving battery and reducing unnecessary RF output.

---

## 8) Troubleshooting patterns

### Symptom: Bound, but no stick movement in Betaflight
- Ports: Serial RX enabled on correct UART?
- Config: Serial receiver + CRSF selected?
- CLI: `serialrx_inverted` and `serialrx_halfduplex` OFF?
- Wiring: RX ↔ TX swapped correctly?

### Symptom: Won’t bind after update
- Confirm **major versions match** TX and RX.
- Confirm binding phrase matches (or clear it on both).
- Confirm regulatory domain settings consistent with your region and gear.

### Symptom: “WiFi update won’t start”
- Some receivers require a specific power-cycle pattern or a timed hold state.
- If WiFi credentials were never set, fallback to UART/passthrough.

---

## References (recommended)
Official:
- ExpressLRS receiver wiring: https://www.expresslrs.org/quick-start/receivers/wiring-up/
- ExpressLRS FC configuration (CRSF settings): https://www.expresslrs.org/quick-start/receivers/configuring-fc/
- Betaflight passthrough update method: https://www.expresslrs.org/software/updating/betaflight-passthrough/
- ELRS updating overview: https://www.expresslrs.org/quick-start/receivers/updating/

Practical:
- Oscar Liang ELRS setup guide: https://oscarliang.com/setup-expresslrs-2-4ghz/
