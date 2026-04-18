---
layout: default
title: OTA Updates
nav_order: 9
description: "Development and production over-the-air firmware update support."
---

# OTA Updates

## Development Mode (Arduino OTA)
- Enables direct PlatformIO upload OTA
- Must opt-in in `Settings -> Miscellaneous`
- Intended for firmware development

## Production Mode (esp32FOTA)
- Smooth OTA process through web UI
- Supports markdown Changelogs for end user
- HTTP/HTTPS firmware downloads
- RSA signature verification with public key
- Firmware manifest URL for version checking
- Progress callbacks for UI updates
- Automatic rollback on boot failure
- Build script to generate binaries and json

---

[← Previous: Security](security.md) \| [Next: Home Assistant →](home-assistant.md)
