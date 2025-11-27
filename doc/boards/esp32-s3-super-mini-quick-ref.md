# ESP32-S3 Super Mini - Quick Reference Card

## 🚀 Getting Started

### Hardware Setup
1. Connect LED matrix DIN to GPIO18
2. Connect power (5V USB or external PSU)
3. Common GND for all components

### Software Setup
1. Open PlatformIO
2. Edit `platformio.ini`:
   ```ini
   default_envs = esp32-s3-super-mini-LED-32x8
   ```
3. Build and upload

## 📌 Essential Pins (Quick Reference)

| Function | Pin | Notes |
|----------|-----|-------|
| 💡 LED Matrix | GPIO18 | WS281x DIN |
| ☀️ Light Sensor | GPIO1 (A0) | Analog |
| 🌡️ DHT Sensor | GPIO5 | Digital |
| 🔗 I2C SDA | GPIO8 | Pull-up |
| 🔗 I2C SCL | GPIO9 | Pull-up |
| 🔊 I2S WS | GPIO15 | Optional |
| 🔊 I2S SCK | GPIO16 | Optional |
| 🔊 I2S DIN | GPIO17 | Optional |
| 🔘 Button | GPIO0 | Boot btn |
| 🌈 RGB LED | GPIO48 | Onboard |

## ⚡ Power

```
USB-C: 5V input
3V3 pin: 3.3V output (max ~500mA)
GND: Common ground

For LED matrices > 8 LEDs: Use external 5V PSU
```

## 🔧 Build Configuration

**Environment:** `esp32-s3-super-mini-LED-32x8`
**MCU:** ESP32-S3 @ 240MHz
**Flash:** 4MB
**Build Mode:** Small (optimized)
**Display:** LED Matrix 32x8 (column-major alternating)

## ⚠️ Critical Warnings

**NEVER USE:**
- GPIO9-12 (Flash memory)
- GPIO19-20 (USB D+/D-)

**USE WITH CAUTION:**
- GPIO0 (Boot/strapping - OK for button)
- GPIO3 (JTAG strapping)
- GPIO48 (Shared with onboard LEDs)

## 🔌 Typical Wiring (32x8 Matrix)

```
ESP32-S3 Super Mini          Components
────────────────────         ──────────

GPIO18 ──────────────────→  LED Matrix DIN
GPIO1 (A0) ───┬──────────→  LDR ── 1kΩ ── 3.3V
              └──────────→  LDR ────────── GND
GPIO5 ────────────────────→  DHT11 DATA
GPIO8 ────────────────────→  I2C SDA
GPIO9 ────────────────────→  I2C SCL
GPIO0 ────────────────────→  Button (to GND)

5V ───────────────────────→  LED Matrix VCC (or ext PSU)
3.3V ─────────────────────→  Sensors VCC
GND ──────────────────────→  All GND (common)
```

## 📚 Documentation

- [Full Specifications](./esp32-s3-super-mini.md)
- [Detailed Pinout](./esp32-s3-super-mini-pinout.md)
- [Board Config](../../config/board.ini) (search: `esp32-s3-super-mini`)
- [Main Board README](./README.md)

## 💻 PlatformIO Commands

```bash
# Build for ESP32-S3 Super Mini
pio run -e esp32-s3-super-mini-LED-32x8

# Upload
pio run -e esp32-s3-super-mini-LED-32x8 -t upload

# Monitor serial
pio device monitor -b 115200

# Clean build
pio run -e esp32-s3-super-mini-LED-32x8 -t clean
```

## 🐛 Troubleshooting

| Issue | Solution |
|-------|----------|
| Upload fails | Hold BOOT (GPIO0) + press RESET |
| Not detected | Check USB cable supports data |
| Flash memory errors | Never use GPIO9-12 |
| LED matrix doesn't work | Check GPIO18 connection & power |
| I2C devices not found | Check pull-ups, verify address |

## 📖 Specifications Summary

| Item | Value |
|------|-------|
| **CPU** | Dual Xtensa LX7 @ 240MHz |
| **Flash** | 4 MB |
| **SRAM** | 512 KB |
| **WiFi** | 802.11 b/g/n |
| **Bluetooth** | 5.0 LE |
| **USB** | Native USB-C |
| **Size** | 22.52 x 18 mm |
| **ADC Pins** | 6 (10-bit) |
| **GPIO** | 27 total (11 accessible) |

---

**Board Profile Created:** November 2025  
**Pixelix Version:** Compatible with current master branch  
**PlatformIO Environment:** `esp32-s3-super-mini-LED-32x8`
