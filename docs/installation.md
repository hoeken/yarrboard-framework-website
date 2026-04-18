---
layout: default
title: Installation
nav_order: 4
description: "Installing the library via PlatformIO, build system configuration, and dependencies."
---

# Installation

## PlatformIO Registry

Install the library from the PlatformIO Registry by adding it to your `platformio.ini`:

```ini
[env:myenv]
lib_deps =
    hoeken/YarrboardFramework
```

Or install via the PlatformIO CLI:

```bash
pio pkg install --library "hoeken/YarrboardFramework"
```

## Build System

### PlatformIO Configuration

```ini
[platformio]
lib_dir = .
src_dir = src
default_envs = default

[env]
platform = espressif32
framework = arduino
board = esp32-s3-devkitc-1
board_build.partitions = default_16MB.csv
board_build.filesystem = littlefs

lib_deps =
    hoeken/YarrboardFramework
    bblanchon/ArduinoJson
    hoeken/PsychicHttp
    # Additional dependencies as needed

extra_scripts =
    pre:scripts/gulp.py          # Build web assets
    pre:scripts/git_version.py   # Embed git version
```

### Node.js

Run ```npm install``` in order to install gulp and related tools for building the static html + javascript + css + image assets as files that can be embedded directly into the firmware.

### Dependencies

Core libraries managed via PlatformIO:
- **ArduinoJson** - JSON parsing and serialization
- **PsychicHttp** - Async HTTP/WebSocket server
- **PsychicMqttClient** - MQTT client
- **FastLED** - RGB LED control
- **ETL (Embedded Template Library)** - Fixed-size containers without dynamic allocation
- **esp32FOTA** - OTA firmware updates with signing
- **improv** - WiFi provisioning protocol

---

[← Previous: Frontend Architecture](frontend-architecture.md) \| [Next: Usage Guide →](usage.md)
