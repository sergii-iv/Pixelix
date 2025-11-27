# ESP32-S3 Super Mini - Pin Mapping for Pixelix

## Pin Assignment Summary

```
┌─────────────────────────────────────┐
│      ESP32-S3 Super Mini            │
│                                     │
│  [USB-C]  ┌──────────┐ [WS2812]    │
│           │ ESP32-S3 │              │
│           │  240MHz  │              │
│           │  4MB     │              │
│           └──────────┘              │
└─────────────────────────────────────┘

Left Side:                Right Side:
5V ────────────────       ────────────── GND
GND ───────────────       ────────────── 3V3
IO1 (A0)  LDR     ───     ────────────── IO21
IO2 (A1)          ───     ────────────── IO20 (RX)
IO3 (A2)          ───     ────────────── IO19 (TX)
IO4               ───     ────────────── IO18  LED_MATRIX_OUT
IO5       DHT     ───     ────────────── IO17  I2S_DI
IO6               ───     ────────────── IO16  I2S_SC
IO7               ───     ────────────── IO15  I2S_WS
IO8       I2C_SDA ───     ────────────── IO14
IO9       I2C_SCL ───     ────────────── IO13
IO10              ───     ────────────── IO12
IO11              ───     ────────────── IO11
                          (mirrored)

Bottom:
Boot Button (IO0)
WS2812 RGB LED (IO48)
```

## Configuration Details

| **Category** | **Function** | **Pin** | **Direction** | **Configuration** |
|-------------|-------------|---------|---------------|-------------------|
| **Display** | LED Matrix Data | GPIO18 | Output | WS281x data line |
| **Sensors** | LDR (Light) | GPIO1 | Analog In | GL5528 + 1kΩ resistor |
| | DHT Temp/Humid | GPIO5 | Digital I/O | DHT11 |
| **I2C** | SDA | GPIO8 | Bidirectional | Pull-up required |
| | SCL | GPIO9 | Bidirectional | Pull-up required |
| **I2S** | Word Select (WS) | GPIO15 | Output | Audio (optional) |
| | Serial Clock (SC) | GPIO16 | Output | Audio (optional) |
| | Data In (DI) | GPIO17 | Input | Audio (optional) |
| **User Input** | OK Button | GPIO0 | Input | Pull-up, Boot button |
| **Indicators** | Onboard RGB LED | GPIO48 | Output | WS2812 |
| **Power** | 5V Input | 5V | Power | USB-C |
| | 3.3V Output | 3V3 | Power | Regulated |
| | Ground | GND | Ground | Common ground |

## Build Configuration

**Environment name:** `esp32-s3-super-mini-LED-32x8`

**PlatformIO Environment:**
```ini
default_envs = esp32-s3-super-mini-LED-32x8
```

**Board Configuration:** `config/board.ini` → `[board:esp32-s3-super-mini-LED-32x8]`

**Display Configuration:** `config/display.ini` → `[display:led_matrix_column_major_alternating]`

**Build Mode:** `small` (optimized for 4MB flash)

## Wiring Guide for LED Matrix

### 32x8 LED Matrix Connection

```
ESP32-S3 Super Mini          32x8 LED Matrix (WS2812)
─────────────────────        ────────────────────────
      5V ───────────────────→ VCC (or external 5V PSU)
     GND ───────────────────→ GND
   GPIO18 ──────────────────→ DIN (Data In)
```

### External Power Recommendation

For LED matrices larger than a few LEDs, use an external 5V power supply:

```
                    ┌──────────────┐
  External 5V PSU ──┤+            -├── GND
                    └──────────────┘
                         │    │
                         │    └──────→ LED Matrix GND
                         └───────────→ LED Matrix VCC
                                   
  ESP32-S3 GND ──────────────────────→ Common GND
  ESP32-S3 GPIO18 ────────────────────→ LED Matrix DIN
```

> **Important:** Always connect all grounds together (ESP32, LED matrix, and power supply).

## Sensor Connections

### Light Sensor (LDR)

```
     3.3V
      │
      ├──── 1kΩ Resistor ────┬──→ GPIO1 (A0)
                             │
                          ┌──┴──┐
                          │ LDR │ (GL5528)
                          └──┬──┘
                             │
                            GND
```

### DHT11 Temperature/Humidity Sensor

```
DHT11          ESP32-S3 Super Mini
─────          ────────────────────
VCC  ────────→ 3.3V
DATA ────────→ GPIO5 (with 10kΩ pull-up to 3.3V)
NC
GND  ────────→ GND
```

## I2C Device Connection

```
I2C Device         ESP32-S3 Super Mini
──────────         ────────────────────
VCC  ────────────→ 3.3V
SDA  ────────────→ GPIO8 (internal pull-up enabled)
SCL  ────────────→ GPIO9 (internal pull-up enabled)
GND  ────────────→ GND
```

Common I2C devices:
- SHT3x temperature/humidity sensor
- DS3231 RTC module
- OLED displays
- Other I2C peripherals

## Programming and Debugging

### Uploading Firmware

1. Connect USB-C cable
2. The board should be recognized automatically (native USB)
3. Press and hold BOOT button (GPIO0) + press RESET if needed to enter bootloader mode
4. Upload via PlatformIO or Arduino IDE

### Serial Monitor

- **Baud Rate:** 115200
- **Port:** Automatically detected (native USB)
- The board has native USB support, so no external USB-to-serial chip is needed

## Notes

- ✅ Ultra-compact 22.52 x 18 mm form factor
- ✅ 4MB flash is sufficient for most Pixelix configurations (using `small` build mode)
- ✅ Native USB-C for programming and power
- ⚠️ GPIO48 controls both WS2812 RGB and red power LED
- ⚠️ GPIO9-12 are used for flash - never use these pins
- ⚠️ GPIO19-20 are used for USB - never use these pins
- ⚠️ No PSRAM in standard version (512KB SRAM only)
