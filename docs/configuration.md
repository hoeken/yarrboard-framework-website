---
layout: default
title: App Configuration
nav_order: 6
description: "Loading and validating JSON configuration via controller hooks."
---

# App Configuration

The application configuration is stored in the LittleFS partition file `/yarrboard.json`. This file contains all configuration data for the entire board.  YBF provides lifecycle hooks to allow you to load / save the configuration data.

## Configuration Loading

Controllers load configuration via hooks with error handling:

```cpp
bool MyController::loadConfigHook(JsonVariant config, char* error, size_t len) {
  // Access nested configuration
  setting1 = config["setting1"] | defaultValue;

  // Validate and report errors
  if (setting1 < 0 || setting1 > 100) {
    snprintf(error, len, "setting1 must be between 0 and 100");
    return false;
  }

  return true;
}
```

Very simple hook for generating the config json:

```cpp
void MyController::generateConfigHook(JsonVariant output)
{
  output["setting1"] = this.setting1;
};
```

Capabilities hook is for describing the static hardware capabilities of your device:

```cpp
void MyController::generateCapabilitiesHook(JsonVariant config)
{
  config["modbus"] = true;
}
```

---

[← Previous: Usage Guide](usage.md) \| [Next: Protocol Reference →](protocol.md)
