---
layout: default
title: Configuration
nav_order: 5
description: "Loading and validating JSON configuration via controller hooks."
---

# Configuration

## Configuration Access

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

---

[← Previous: Usage Guide](usage.md) \| [Next: Protocol Reference →](protocol.md)
