# Corne V3 — ZMK Config

*A wireless, per‑key‑RGB, OLED‑equipped split keyboard powered by nice!nano v2 (compatible)*

[![Build](https://github.com/your‑github‑handle/zmk‑config/actions/workflows/build.yaml/badge.svg)](../../actions) 
[![License](https://img.shields.io/github/license/your‑github‑handle/zmk‑config)](LICENSE) 

---

## ✨ Overview

This repository contains the **complete ZMK firmware configuration** for my custom **Corne V3** split keyboard:

* **MCUs:** 2× nice!nano v2 (nRF52840, BLE 5.0, Li‑Po charging)
* **Switch matrix:** 3×6 + 3 (per half)
* **Lighting:** 42 × WS2812 per‑key LEDs + 12 × underglow (GRB)
* **Display:** 0.91 “ 128×32 SSD1306 OLED on each half
* **Power:** 302 mAh Li‑Po, magnetic charging pogo‑pins
* **Host OS:** macOS 15, but any BLE‑enabled OS works

The firmware is built with **ZMK $Zephyr Mechanical Keyboard$** and provides:

* True wireless split (BLE link between halves, no TRRS needed)
* Up to 5 easily switchable host profiles
* Layer & output status on the OLED
* Comprehensive RGB underglow / per‑key effects (breathe, cycle, reactive)
* VIA‑style live keymap editing with **ZMK Studio**

---

## 📦 Hardware Map

| Half               | MCU          | I/O pins                                                                                                            |
| ------------------ | ------------ | ------------------------------------------------------------------------------------------------------------------- |
| Left (central)     | nice!nano v2 | Column 0‑5 → P1.06 … P1.15  <br/> Row 0‑3 → P0.13 … P0.16 <br/> OLED I²C → P0.02 / P0.03 <br/> RGB DIN → P0.28 (D4) |
| Right (peripheral) | nice!nano v2 | Mirrored pins; identical wiring                                                                                     |

---

## 🚀 Getting Started

### 1. Install ZMK & init west

https://zmk.dev/docs/user-setup

### 2. Build firmware

```bash
# Build central (left)
west build -p -b nice_nano_v2 -d build/left  -- -DSHIELD="corne_left nice_view_adapter nice_view" -DSNIPPET="studio-rpc-usb-uart" -DCONFIG_ZMK_STUDIO=y
# Build peripheral (right)
west build -p -b nice_nano_v2 -d build/right -- -DSHIELD="corne_right nice_view_adapter nice_view"
```

### 3. Flash

1. Double‑tap **RESET** on the MCU → it mounts as `NICENANO`.
2. Copy `build/zephyr/zmk.uf2` onto the drive.
3. Repeat for the other half.

---

## 🔌 Bluetooth Workflow

* **Pairing mode:** `Ctrl + Shift + J` (hold >3 s) — host sees *“ZMK‑Corne”*.
* **Cycle profiles:** `Ctrl + Shift + K`.
* **Erase profiles:** `Ctrl + Shift + P` (long‑hold).
* **Peripheral ↔ central link** is automatic after power‑on (blue LED solid).

---

## 💡 RGB Controls

Keybindings (Layer FN):

```
RGB_TOG  RGB_HUI  RGB_SAI  RGB_BRI  RGB_EFF
```

* Default effect: **Breathe**
* Underglow & per‑key share the same effect engine but can be toggled independently in `prj.conf`.

---

## 📺 OLED Widgets

| Widget        | Purpose                            |
| ------------- | ---------------------------------- |
| Layer Status  | Shows active layer name/number     |
| Battery Gauge | Per‑half Li‑Po voltage & icon      |
| Output Status | USB/BLE profile & caps/scroll lock |

Widgets blank after 15 s of idle to save \~15 mA.

---

## 🗺️ Layers (quick view)

|  #  | Name       | Use‑case                |
| :-: | ---------- | ----------------------- |
|  0  | **Base**   | Colemak‑DH              |
|  1  | **Lower**  | Symbols & navigation    |
|  2  | **Raise**  | Numbers & function keys |
|  3  | **Adjust** | RGB, bootloader, Studio |
|  4  | **Music**  | MIDI chords (optional)  |

Detailed `.keymap` lives in `/config/corne.keymap` and is editable via **Keymap Editor** or ZMK Studio.

---

## 🔋 Power Tips

* Average typing current: **7 mA** with OLED off, RGB off.
  → \~40 h on a 302 mAh cell.
* Enable `CONFIG_ZMK_RGB_SLEEP` & `CONFIG_ZMK_DISPLAY_BLANK_ON_IDLE` (already set) for multi‑week standby.
* Charging LED on nice!nano v2 turns green at \~4.2 V.

---

## 🧩 Extending

* **Add encoders:** see `/docs/encoders.md` for GPIO mapping.
* **Dongle mode:** flash a third nRF board with `central_dongle` target and leave it in a USB port for faster resume.
* **QMK port:** experimental RP2040 branch lives in `feature/qmk‑rp2040` — still WIP.

---

## 📚 Resources

* [ZMK Docs](https://zmk.dev/docs)
* [Keymap Editor](https://nickcoutsos.github.io/keymap-editor)
* [ZMK Studio](https://zmk.studio)

---

> **Made with ☕ by @lysandroc**

