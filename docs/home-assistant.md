---
layout: default
title: Home Assistant
nav_order: 9
description: "Automatic MQTT discovery, topic structure, and device information for Home Assistant integration."
---

# Home Assistant Integration

## Automatic Discovery

Controllers and channels automatically publish MQTT discovery messages:

```cpp
void MyChannel::haGenerateDiscovery(JsonVariant doc, const char* uuid, MQTTController* mqtt) {
  // Populate discovery document
  doc["name"] = name;
  doc["state_topic"] = "~/state";
  doc["command_topic"] = "~/set";
  doc["device_class"] = "switch";
  doc["unique_id"] = ha_uuid;

  // Framework handles device info, availability, etc.
  mqtt->publishDiscovery("switch", key, uuid, doc);
}
```

## MQTT Topics

Hierarchical topic structure:
```
yarrboard/{hostname}/state          # Device state
yarrboard/{hostname}/availability   # Online/offline status
yarrboard/{hostname}/channel1/state # Channel-specific state
```

## Device Information

Automatically included in all discovery messages:
- Device name, manufacturer, model
- Hardware and firmware versions
- Configuration and support URLs
- Unique identifiers

---

[← Previous: OTA Updates](ota.md) \| [Next: Hardware & Performance →](hardware.md)
