---
layout: default
title: OTA Updates
nav_order: 9
description: "Development and production over-the-air firmware update support."
---

# OTA Updates

## Development Mode (Arduino OTA)
- Enabled via PlatformIO upload
- Password-protected
- mDNS-based discovery
- Used during firmware development

## Production Mode (esp32FOTA)
- HTTP/HTTPS firmware downloads
- RSA signature verification with public key
- Firmware manifest URL for version checking
- Progress callbacks for UI updates
- Automatic rollback on boot failure

## Firmware Manifest
```json
{
  "version": "1.2.0",
  "url": "https://example.com/firmware.bin",
  "signature": "base64-encoded-signature"
}
```

---

[← Previous: Security](security.md) \| [Next: Home Assistant →](home-assistant.md)
