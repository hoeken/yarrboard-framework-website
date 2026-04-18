---
layout: default
title: Hardware & Performance
nav_order: 10
description: "System limits, performance monitoring, supported ESP32 targets, and memory usage."
---

# Hardware & Performance

## System Limits

| Limit | Default | Configurable |
|-------|---------|--------------|
| Maximum controllers | 30 | `YB_MAX_CONTROLLERS` |
| Maximum protocol commands | 50 | `YB_PROTOCOL_MAX_COMMANDS` |
| Maximum HTTP clients | 13 | ESP-IDF limit |
| WebSocket message queue | 100 messages | `HTTPController` |

## Performance Monitoring

Built-in profiling via `IntervalTimer`:
- Per-controller loop execution time
- Rolling average over configurable window
- Accessible via stats API and web interface
- Framerate calculation (main loop Hz)

## Hardware Support

### Primary Target
- **ESP32-S3**: Full support including USB CDC and Bluetooth Improv
- 16MB flash recommended (8MB minimum)
- PSRAM optional

### Memory Usage

The basic requirements for Yarrboard Framework are an ESP32 S3 board with at least 8mb flash.

Even though the compiled firmware is only about 2mb, due to the nature of OTA, you need 2 partitions that can each hold the full firmware.  Additionally, the board configuration json file is stored in a LittleFS partition.

#### Minimal

**Flash:** ~1.9MB
**RAM:** ~323kib (201kib free heap after boot on ESP32-S3)

#### Standard (with NimBLE)

Adding the NimBLE bluetooth stack for provisioning ImprovWifi over bluetooth requires additional memory and flash space:

**Flash:** ~2.1MB
**RAM:** ~345kib (179kib free heap after boot on ESP32-S3)


### Supported Targets

* ESP32 (tested)
* ESP32-S3 (tested)
* ESP32-C3
* ESP32-C5
* ESP32-C6
* ESP32-S2

The firmware compiles for the other targets, but has not been tested yet.

### Peripherals
- **RGB LED**: FastLED-compatible (WS2812, etc.)
- **Buzzer**: PWM-capable piezo buzzer
- **Channels**: GPIO, I2C, SPI, ADC, etc. (application-defined)

---

[← Previous: Home Assistant](home-assistant.md) \| [Next: Debugging & Logging →](debugging.md)
