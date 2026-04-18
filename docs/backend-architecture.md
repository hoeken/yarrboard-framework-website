---
layout: default
title: Backend Architecture
nav_order: 3
description: "Controller-based design, built-in controllers, channel system, and design philosophy."
---

# Backend Architecture

## Controller-Based Design

All subsystems inherit from `BaseController`, providing a unified lifecycle:

```cpp
class BaseController {
  virtual bool setup();                                                                         // Initialization
  virtual void loop();                                                                          // Main execution
  virtual bool loadConfigHook(JsonVariant config, char* error, size_t len);                    // Load configuration with error handling
  virtual void generateConfigHook(JsonVariant config);                                         // Serialize configuration
  virtual void generateCapabilitiesHook(JsonVariant capabilities);                             // Report hardware capabilities
  virtual void generateUpdateHook(JsonVariant output);                                         // Real-time data updates
  virtual bool needsFastUpdate();                                                              // Check if fast update needed
  virtual void generateFastUpdateHook(JsonVariant output);                                     // Fast real-time updates
  virtual void generateStatsHook(JsonVariant output);                                          // Statistics generation
  virtual void mqttUpdateHook(MQTTController* mqtt);                                           // MQTT publishing
  virtual void haUpdateHook(MQTTController* mqtt);                                             // Home Assistant state updates
  virtual void haGenerateDiscoveryHook(JsonVariant components, const char* uuid, MQTTController* mqtt); // Home Assistant discovery
  virtual void updateBrightnessHook(float brightness);                                         // Global brightness changes
};
```

## Built-in Controllers

| Controller | Purpose |
|------------|---------|
| `NetworkController` | WiFi (AP/Client), Improv provisioning, mDNS, DNS server |
| `HTTPController` | Async HTTP/HTTPS server, WebSocket support, client management |
| `ProtocolController` | JSON command protocol with role-based access control |
| `AuthController` | Three-tier role system with session management |
| `MQTTController` | MQTT client with Home Assistant discovery |
| `OTAController` | Arduino OTA (dev) and esp32FOTA (production) with signed firmware |
| `ConfigManager` | JSON configuration storage in LittleFS with validation |
| `DebugController` | Core dumps, logging, IntervalTimer profiling |
| `NTPController` | Network time synchronization |
| `BuzzerController` | PWM piezo buzzer with melody system |
| `RGBController` | FastLED integration with status indication |
| `NavicoController` | UDP multicast publishing for marine electronics |

## Channel System

Template-based hardware abstraction for I/O management:

```cpp
template<typename ChannelType, uint8_t COUNT>
class ChannelController : public BaseController {
  etl::vector<ChannelType, COUNT> channels;
  // Automatic configuration, updates, MQTT, and Home Assistant integration
};
```

Channels inherit from `BaseChannel` and implement:
- `init(uint8_t id)` - Channel initialization with numeric ID (1-N)
- `loadConfig(JsonVariantConst config, char* error, size_t err_size)` - Configuration loading with validation and error reporting
- `generateConfig(JsonVariant config)` - Serialize channel configuration
- `generateUpdate(JsonVariant output)` - Real-time status updates
- `generateStats(JsonVariant output)` - Statistics generation
- `haGenerateDiscovery(JsonVariant doc, const char* uuid, MQTTController* mqtt)` - Home Assistant discovery message
- `haPublishState(MQTTController* mqtt)` - Publish state to Home Assistant
- `haPublishAvailable(MQTTController* mqtt)` - Publish availability to Home Assistant

## Design Philosophy

### Principles

1. **Modularity**: Everything is a controller that can be added or removed
2. **Extensibility**: Easy to add custom controllers and protocol commands
3. **Configuration-Driven**: All settings in JSON, editable via web UI
4. **Real-time First**: WebSocket-based live updates for responsive UIs
5. **Type Safety**: ETL for fixed-size containers without heap allocation
6. **Embedded-Friendly**: Careful memory management, no exceptions

### Architectural Patterns

- **Controller Pattern**: All subsystems inherit from `BaseController`
- **Template-Based Channels**: Type-safe hardware abstraction
- **Hook-Based Extension**: Virtual functions for customization
- **JSON Everything**: Configuration, commands, and data transfer

### Constraints

- **Single-Threaded Main Loop**: Must not block
- **No Exceptions**: Embedded-friendly error handling
- **ESP32-S3 Focus**: Other ESP32 variants supported but not primary target

---

[← Previous: Frontend Architecture](frontend-architecture.md) \| [Next: Installation →](installation.md)
