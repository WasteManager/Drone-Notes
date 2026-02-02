# ExpressLRS (ELRS) How-To: Definitive Getting Started Guide

This guide turns ELRS from “why is this weird?” into a repeatable setup workflow: install the right tools, flash firmware correctly, wire/configure your receiver, and verify everything is talking.

---

## What ELRS is (and why people use it)

**ExpressLRS (ELRS)** is a **high-performance, low-latency, long-range** RC control link:

- **Modern feature set:** telemetry, Lua configuration, Betaflight integration
- **Very low latency:** commonly ~4–6 ms (or lower depending on settings)
- **Very long range:** far beyond typical legacy 1–2 km links (range depends on packet rate, power, environment, antennas, etc.)
- **Open source ecosystem:** many vendors make compatible hardware → lots of choice, usually cheaper, and broadly available

**Downside:** it’s a bit more technical up front—mainly because **flashing firmware is part of normal life** with ELRS.

---

## You need these things

### Hardware
- A **radio** with:
  - an **internal** ELRS module (built-in), or
  - an **external** ELRS module (JR bay / Nano / Lite, etc.)
- An **ELRS receiver** installed in your aircraft
- A flight controller (Betaflight assumed below)

### Software
- **ExpressLRS Configurator** (desktop app)
- **ExpressLRS Lua script** on your radio’s SD card (usually `ELRSv2.lua`)

---

## Step 1 — Install ExpressLRS Configurator

1. Download and install **ExpressLRS Configurator** for your OS.
2. Plan to **flash both your TX module and RX** so you *know* they match firmware and options.

Why you’re doing this:
- ELRS updates frequently.
- New gear often ships with unknown/older firmware.
- Critical settings (like **binding phrase**) are applied through flashing.

---

## Step 2 — Identify the correct firmware “Target”

In ELRS Configurator you must select a **Target** (your specific hardware) before building/flashing.

### How to find the right Target
- In Configurator:
  - Select **Device Category** (vendor/platform)
  - Then select the correct **Device** (exact module/receiver)
- If unsure:
  - Check the manufacturer’s product page; many list the firmware target name.
  - Usually searching the vendor name narrows it down to only a few options (TX module, RX receiver, etc.).

> **Important:** ELRS comes in **2.4 GHz** and **900 MHz**. Make sure your target matches your hardware band.

---

## Step 3 — Install the ExpressLRS Lua Script on your radio

The Lua script is how you configure ELRS from the radio (Wi-Fi, packet rate, telemetry ratio, power, etc.).

### Check if you already have it
On your radio:
1. Power on
2. Go to **Tools** menu (varies by radio; often long-press `SYS`)
3. Look for **“ExpressLRS”** script

You might also see an **older** script called **“ELRS”** (from ELRS 1.x). Ignore it unless you’re deliberately running ancient firmware.

### If you need to install/update the script
1. Connect radio to PC via USB **data** port (not charge-only port).
2. Choose **USB Storage (SD)** on the radio prompt.
3. In ELRS Configurator, select your **radio/module target**.
4. Click **Download Lua Script** (typically downloads `ELRSv2.lua`).
5. Copy it to your radio SD card:
   - `SDCARD/SCRIPTS/TOOLS/ELRSv2.lua`

---

## Step 4 — Configure your radio to talk to ELRS (CRSF)

ELRS uses the **CRSF (Crossfire) protocol** between the radio and the module.

### Internal module radios (built-in ELRS)
- Set **Internal RF** = **CRSF**

### External module radios
- Set **Internal RF** = **OFF**
- Set **External RF** = **CRSF**

### Verify the module is alive
Run the **ExpressLRS Lua** script:
- Options should populate
- You should see the module name (upper left)
- When a bound receiver is powered, you’ll also see **Connected** (often “C” indicator), and the receiver LED should go solid.

---

## Step 5 — Wire the ELRS receiver to the flight controller (same as Crossfire)

ELRS receiver wiring matches Crossfire wiring.

Receiver pins typically include:
- **TX**
- **RX**
- **5V / VCC**
- **GND**

### Correct wiring rule (cross TX/RX)
- Receiver **TX** → FC **RX** (on chosen UART)
- Receiver **RX** → FC **TX** (on chosen UART)
- 5V → 5V, GND → GND

---

## Step 6 — Betaflight configuration (Ports + Receiver + Telemetry)

### Ports tab
- On the UART you wired the receiver to:
  - Enable **Serial RX**

### Receiver / Configuration tab (depends on BF version)
- **Serial Receiver Provider / Protocol:** **CRSF**
- Enable **Telemetry** feature (recommended; required for some functions like dynamic power)

---

## Step 7 — Understand ELRS “binding” (it’s different)

Many ELRS receivers/modules don’t use a classic bind button workflow.

### ELRS binding = **Binding Phrase**
- Any TX module + RX receiver that share the **same binding phrase** will bind.
- Huge benefit: swapping radios/modules is easy—flash the new hardware with the same phrase and everything links.

**Downside:** to set the binding phrase, you usually **must flash firmware**.

---

## Step 8 — Set the basic ELRS options before flashing

In ELRS Configurator, set these at minimum:

### Regulatory Domain
- **EU:** `EU_CE_2400`
- **Most other regions:** `ISM_2400`

### Binding Phrase
- Pick a phrase you will use consistently on all your ELRS gear.
- The phrase is not primarily for security—just a “match key” for binding.

> Most other options can stay default while you’re getting airborne.

---

# Flashing Firmware: The practical methods

## Method 1 — EdgeTX Passthrough (internal module radios)

Use this when your radio has a built-in ELRS module.

1. Plug USB into the radio.
2. Choose **USB Serial Debug** (not USB Storage).
3. In ELRS Configurator:
   - Select correct target
   - Choose **EdgeTX Passthrough**
4. Click **Build & Flash**
5. Do **not** unplug during flashing.

> First-ever build may download dependencies (PlatformIO, etc.). After that it’s faster.

---

## Method 2 — Direct USB on an external module (UART flashing)

Use this when your external module has a USB port.

1. Plug the module into USB.
2. In ELRS Configurator:
   - Select correct target
   - Flash method: **UART**
   - Select the correct **COM port**
3. Click **Build & Flash**

### Windows driver note
Many modules require **Silicon Labs CP210x (CP2102/CP210x) USB-to-UART drivers** on Windows.

**COM port tip:** unplug the module and watch which COM port disappears; that’s the one.

---

## Method 3 — Wi-Fi “Access Point” (device creates its own Wi-Fi)

Great when the module/receiver has no accessible USB port.

### TX module AP flashing flow
1. In Configurator, select correct target + options.
2. Click **Build** (not Build & Flash).
3. Save the generated **`.bin` firmware** somewhere easy.
4. On radio, open ELRS Lua → **WiFi Connectivity** → **Enable WiFi**
5. On your phone/PC, connect to the Wi-Fi network:
   - Usually `ExpressLRS TX`
6. Open browser:
   - Go to **`http://10.0.0.1`**
7. Use **Firmware Update** → select the `.bin` → **Update**

### “Targets Mismatch” warning (common gotcha)
If you enabled Wi-Fi on the **wrong module** (e.g., internal instead of external), the web page will warn you.

Fix:
- For external module use:
  - **Internal RF = OFF**
  - **External RF = CRSF**
Then re-enable Wi-Fi from the Lua script.

---

## Method 4 — Join your Home Wi-Fi (easiest day-to-day)

Once your device has your home Wi-Fi credentials, it can appear directly in ELRS Configurator over the network.

Two ways to set Wi-Fi credentials:
- **Bake into firmware** (set in Configurator before flashing), or
- Connect to `10.0.0.1` while in AP mode and enter SSID/password on the web page

Once connected:
- Your devices show up in ELRS Configurator under **Wi-Fi devices**
- You can select one and **Build & Flash** without cables/drivers

Also, you can often reach the device via a local hostname (commonly something like `elrs_tx.local` / `elrs_rx.local`) depending on network/mDNS support.

---

## Method 5 — Flash the receiver via Wi-Fi (recommended)

### Receiver Wi-Fi behavior
If a receiver powers up and **does not see a bound transmitter**, after ~20–30 seconds it will enter Wi-Fi mode (often indicated by faster blinking).

1. Power the quad (receiver powered).
2. Wait for Wi-Fi mode.
3. Connect your phone/PC to:
   - Usually `ExpressLRS RX`
4. Open:
   - **`http://10.0.0.1`**
5. Choose the **RX firmware `.bin`** (not TX!)
6. Update firmware

> Older firmware may not show a progress bar; wait for “update success / rebooting”.

---

## Method 6 — Betaflight passthrough to receiver (avoid if possible)

This sends firmware through the FC to the receiver.

It can work, but it’s easy to get snagged by requirements like:
- bootloader pad bridges (first flash vs later flashes)
- UART settings issues (inversion / half-duplex)
- telemetry conflicts
- FC-specific quirks

If Wi-Fi is available, **use Wi-Fi**.

---

## Recovery Method — FTDI flashing (unbrick)

If you interrupt flashing and the device appears “dead,” it’s often recoverable via **FTDI** flashing.

You typically only need this if:
- device won’t boot normally
- Wi-Fi doesn’t come up
- USB flashing fails completely

(Keep this in your back pocket; it’s not a normal workflow.)

---

# Post-setup: Key ELRS settings you should understand

These are adjusted from the **ELRS Lua script** (or on some modules with their own screen/joystick).

## Packet Rate (latency vs range tradeoff)
- Higher packet rate (e.g., **500 Hz**):
  - **lowest latency**
  - **shorter range** (still solid for most people)
- Lower packet rate (e.g., **50–150 Hz**):
  - **more range**
  - **higher latency**

Pick based on your flying style and range needs.

## Telemetry Ratio (fix “sensor lost”)
Telemetry ratio controls how often telemetry packets are sent back.

If you see “sensor lost” while flying:
- Increase telemetry frequency (e.g., from **1:128** to **1:64** or **1:32**)

You can also disable telemetry for maximum consistency, but you lose telemetry features.

## Switch Mode: Hybrid vs Wide (strong recommendation: Wide)
**Hybrid** can make some switches behave unexpectedly (e.g., missing middle positions).

Set:
- **Switch Mode = WIDE**

This gives AUX channels enough resolution for normal use.

## Critical quirk: Arm must be AUX1, high = armed
ELRS expects:
- **AUX1 = ARM**
- **High position = armed**

In Betaflight:
- Modes tab → set ARM to **AUX1**
- Ensure the active range corresponds to **high**

If you don’t follow this, you can run into weird/annoying behavior.

## TX Power + Dynamic Power
- Higher TX power = more range, more battery use.
- **Dynamic Power** adjusts power based on link conditions/distance.
  - Requires **telemetry enabled**.

---

# Quick verification checklist

## Radio / Module
- [ ] Internal RF = CRSF (internal module) **or** External RF = CRSF (external module)
- [ ] ELRS Lua script runs and shows module name
- [ ] Packet rate chosen appropriately (start simple: defaults are fine)

## Receiver / FC
- [ ] Wired TX→RX and RX→TX correctly on a UART
- [ ] Betaflight Ports: Serial RX enabled on that UART
- [ ] Receiver protocol/provider = CRSF
- [ ] Telemetry enabled (especially if using dynamic power)

## Firmware / Binding
- [ ] TX and RX flashed to compatible major version
- [ ] Same binding phrase on TX and RX
- [ ] Receiver LED indicates link (solid when connected)

---

# Troubleshooting bullets (the common hits)

- **“Targets mismatch” on the web flasher:** you enabled Wi-Fi on the wrong module (internal vs external), or built the wrong firmware target.
- **Can’t find the COM port:** missing CP210x drivers, or wrong cable/USB port.
- **Receiver won’t connect:** binding phrase mismatch, firmware major mismatch, or receiver not actually in CRSF mode in Betaflight.
- **Switch mid positions don’t register:** set **Switch Mode = WIDE**.
- **“Sensor lost” warnings:** increase telemetry ratio frequency (e.g., 1:64 or 1:32).
