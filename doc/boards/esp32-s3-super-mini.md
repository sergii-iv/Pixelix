# ESP32-S3 Super Mini <!-- omit in toc -->

![ESP32-S3 Super Mini](https://www.espboards.dev/img/8PTig1JIlM-300.avif)

## Table of Contents <!-- omit in toc -->

- [Overview](#overview)
- [Specifications](#specifications)
- [Pin Configuration for Pixelix](#pin-configuration-for-pixelix)
  - [LED Matrix Connection](#led-matrix-connection)
  - [Sensors](#sensors)
  - [I2C](#i2c)
  - [I2S Audio (Optional)](#i2s-audio-optional)
  - [Buttons](#buttons)
  - [Onboard LED](#onboard-led)
- [Important Notes](#important-notes)
- [Pin Safety Guide](#pin-safety-guide)
  - [Safe Pins for General Use](#safe-pins-for-general-use)
  - [Pins to Avoid](#pins-to-avoid)
- [Power Considerations](#power-considerations)
- [References](#references)

## Overview

The ESP32-S3 Super Mini is an ultra-compact IoT development board featuring the Espressif ESP32-S3 WiFi/Bluetooth dual-mode chip. With its tiny footprint (22.52 x 18 mm), it's ideal for space-constrained projects while maintaining powerful performance.

**Reference:** [https://www.espboards.dev/esp32/esp32-s3-super-mini/](https://www.espboards.dev/esp32/esp32-s3-super-mini/)

## Specifications

| Feature | Specification |
|---------|--------------|
| **MCU** | ESP32-S3 (Dual-core Xtensa LX7) |
| **Clock Speed** | Up to 240 MHz |
| **Flash Memory** | 4 MB |
| **SRAM** | 512 KB |
| **WiFi** | 802.11 b/g/n (2.4 GHz) |
| **Bluetooth** | 5.0 (LE) |
| **USB** | Native USB-C (Serial/JTAG) |
| **GPIO Pins** | 27 (11 directly accessible) |
| **ADC Pins** | 6 (A0-A5) |
| **PWM Pins** | 11 |
| **Dimensions** | 22.52 x 18 mm |
| **Onboard LED** | WS2812 RGB LED (GPIO48) |
| **Power** | 5V via USB-C, 3.3V regulated output |

## Pin Configuration for Pixelix

The following pin configuration is optimized for the Pixelix LED matrix display project with the ESP32-S3 Super Mini board.

### LED Matrix Connection

| Function | GPIO Pin | Notes |
|----------|----------|-------|
| **LED Matrix Data Out** | GPIO18 | DIN connection to LED matrix |

### Sensors

| Sensor | GPIO Pin | Type | Notes |
|--------|----------|------|-------|
| **Light Sensor (LDR)** | GPIO1 (A0) | Analog Input | GL5528 with 1kΩ series resistor |
| **Temperature/Humidity (DHT)** | GPIO5 | Digital I/O | DHT11 compatible |

### I2C

| Function | GPIO Pin | Notes |
|----------|----------|-------|
| **SDA** | GPIO8 | I2C Data |
| **SCL** | GPIO9 | I2C Clock |

Common I2C devices supported:
- SHT3x temperature/humidity sensors
- RTC modules
- Other I2C peripherals

### I2S Audio (Optional)

| Function | GPIO Pin | Notes |
|----------|----------|-------|
| **WS (Word Select)** | GPIO15 | Left/Right clock |
| **SC (Serial Clock)** | GPIO16 | Bit clock |
| **DI (Data In)** | GPIO17 | Serial data |

### Buttons

| Button | GPIO Pin | Notes |
|--------|----------|-------|
| **OK Button** | GPIO0 | Boot button (shared with bootloader) |

> **Note:** GPIO0 is the onboard BOOT button. It has dual functionality - used for entering bootloader mode during reset and can be used as a user button during normal operation.

### Onboard LED

| LED Type | GPIO Pin | Notes |
|----------|----------|-------|
| **WS2812 RGB LED** | GPIO48 | Programmable onboard LED |

> **Warning:** GPIO48 controls both the WS2812 RGB LED and a red power LED. Any signal sent to GPIO48 may cause both LEDs to flicker or behave unexpectedly due to the hardware design.

## Important Notes

1. **No PSRAM**: This board configuration assumes the standard ESP32-S3 Super Mini with 4MB flash and no PSRAM. If you have a variant with PSRAM, you may need to add the `-D BOARD_HAS_PSRAM` build flag.

2. **USB-C Native**: The board uses native USB (no external USB-to-serial chip). This means:
   - Faster programming and serial communication
   - Lower power consumption
   - Direct USB debugging capabilities

3. **Compact Size**: Due to the ultra-small form factor:
   - Limited number of easily accessible pins
   - May require careful wire management
   - Consider using a breakout board or custom PCB for complex projects

4. **Power Supply**: The board provides 3.3V output for peripherals. Ensure total current draw doesn't exceed the onboard regulator's capacity (~500mA typical).

## Pin Safety Guide

### Safe Pins for General Use

These pins are safe for GPIO usage and won't interfere with boot or flash operations:

- **GPIO1** (A0)
- **GPIO2** (A1)
- **GPIO4** (A2)
- **GPIO5** (A3)
- **GPIO6** (A4)
- **GPIO7** (A5)
- **GPIO8**
- **GPIO15**
- **GPIO16**
- **GPIO17**
- **GPIO18**
- **GPIO21**

### Pins to Avoid

| GPIO | Function | Reason to Avoid |
|------|----------|-----------------|
| GPIO0 | Strapping Pin | Boot mode selection - OK for button with pull-up |
| GPIO3 | Strapping Pin | JTAG interface selection |
| GPIO9-12 | Flash/PSRAM | Connected to flash memory - DO NOT USE |
| GPIO19-20 | USB | Native USB D+/D- - DO NOT USE |
| GPIO43-44 | UART0 | Serial debug TX/RX - avoid unless needed |
| GPIO45-46 | Strapping Pins | VDD_SPI voltage and boot mode |

## Power Considerations

- **Input Voltage**: 5V via USB-C
- **Operating Voltage**: 3.3V (regulated)
- **Maximum Current Draw**: ~500mA from 3.3V regulator
- **LED Matrix Power**: Consider external power supply for large LED matrices
- **Sleep Mode Current**: ~43μA in deep sleep

For Pixelix LED matrix projects, if using more than a small 32x8 matrix, an external 5V power supply is recommended to power the LEDs separately from the development board.

## References

- [ESP32-S3 Super Mini Product Page](https://www.espboards.dev/esp32/esp32-s3-super-mini/)
- [ESP32-S3 Datasheet (Espressif)](https://www.espressif.com/sites/default/files/documentation/esp32-s3_datasheet_en.pdf)
- [Pixelix Board Configuration](../../config/board.ini)
- [Pixelix Display Configuration](../../config/display.ini)
