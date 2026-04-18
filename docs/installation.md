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

### Web Asset Build Process

The framework uses Gulp to process web assets:

1. **HTML/CSS/JS Processing**:
   - Inlines all CSS and JavaScript
   - Minifies HTML, CSS, and JavaScript
   - Encodes images as base64 data URIs
   - Gzip compresses the final output
   - Generates C header files with GulpedFile structures
   - Calculates SHA256 for ETag-based caching

2. **Generated Headers**:
   ```cpp
   // GulpedFile structure definition
   struct GulpedFile {
     const uint8_t* data;      // Pointer to the file data array
     size_t length;            // Length of the data in bytes
     const char* sha256;       // SHA-256 hash as hex string
     const char* filename;     // Original filename (e.g., "logo.png")
     const char* mimetype;     // MIME type (e.g., "image/png")
   };
   ```

3. **Automatic Build**:
   - PlatformIO `pre:` scripts run Gulp automatically before compilation
   - Git version script embeds commit hash and build timestamp
   - No manual build step required

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
