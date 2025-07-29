# Belay Next Gen Variants for Kalico 
*Filament buffer with active feed — two variants: Dual‑Sensor & Linear Potentiometer*

> **TL;DR:**  
> Belay decouples the hotend extruder from the filament feed at the spool side.  
> A second, synchronized extruder feeds a buffer ("Belay").  
> Sensors measure the buffer position and dynamically adjust the Belay ratio → smoother flow, less under/over‑pressure, and more stable extrusion at high speeds.

---

## Features
- **Two operating modes:**
  - **Dual‑Sensor:** Two optical endstops (LOW/HIGH) define buffer zones → Belay speeds up, slows down, or stays neutral.
  - **Linear Potentiometer:** Analog position measurement → continuous adjustment of the Belay ratio.
- **Firmware‑integrated** (Kalico): Ratio control via Belay.py
- **Quick tuning:** Easily adjust thresholds, hysteresis, and ratio curves.

---

## Credits
- **RyanG** – Key input on architecture, feedback, and author of the Belay.py firmware integration.  
  <--! Big Thanks to Ryan! 🙌 -->
- **Annex Engineering** – Original Belay concept and community knowledge on multi‑extruder feeds.  
- **Kalico (formerly DangerKlipper)** – Firmware improvements over Klipper that make Belay integration possible.  
- **Annex & Kalico Communities** – Testing, troubleshooting, and suggestions.

> If anyone is missing or should be credited differently: open an Issue or PR. 🙌

---

## How it works

### Principle
- **Two extruders in firmware:**
  - `extruder`: the standard hotend extruder.
  - `extruder_stepper belay`: located before the spool, after the buffer. It is **synchronized** with the main extruder, but with an adjustable **ratio**.
- **Buffer** (Bowden/PTFE loop) stores filament length.
- **Sensor system** measures buffer position:
  - **Dual‑Sensor:**  
    - **LOW** (under‑fill) → Belay **faster** (ratio > 1.0).  
    - **MID** (OK) → Belay **neutral** (≈1.0).  
    - **HIGH** (over‑fill) → Belay **slower** (ratio < 1.0). 
  - **Linear potentiometer:**  
    - Analog value (0–100 %) is mapped via a curve to the ratio (e.g. 0.8–1.2) → **continuous** regulation.
    - >**WIP**

### Advantages
- Less load fluctuation on the hotend extruder.
- Better layer consistency at high accelerations.
- Fewer filament slips with tight or sticky spools.
- Bigger spools possible without challaging the main extruder - printing with ease.

---

## Mechanics (short overview)
- **Dual‑Sensor:** Two optical endstops (LOW & HIGH) mounted so the MID zone is wide enough.
- **Linear Potentiometer:** 10 kΩ linear potentiometer, mechanically coupled to the buffer (slider or lever).

> **Tip:** Strain‑relieve cables, shield optical sensors from ambient light, and protect potentiometer from dust.

---

## Example wiring (pins)
- **MCU pins (from one build):**  
  - `PA14` → **HIGH** (over‑fill)  
  - `PA15` → **LOW** (under‑fill)  
- **Potentiometer version:**  
  - Wiper → MCU analog input (ADC)
  - Every ADC port works, thermistor port is recommended solution.

> Pins, pull‑ups, and invert settings depend on your board and sensors – verify using `QUERY_BUTTON`/`QUERY_ENDSTOPS` or ADC debug.

---

## License
This project is licensed under the **Creative Commons Attribution-ShareAlike 4.0 International License (CC BY-SA 4.0)**.  
See the [LICENSE](LICENSE) file for details.

---

## Disclaimer ⚠️
- This is a **DIY hardware project** – use it **at your own risk**.  
- **No hot‑plugging:** never connect or disconnect cables while the printer is powered on.  
- Incorrect wiring or connections under power can **damage electronics or cause injury**.  
- Always disconnect power before changing wiring, sensors, or boards.  
- The author assumes **no liability** for damage, injury, or loss resulting from the use of this project.
