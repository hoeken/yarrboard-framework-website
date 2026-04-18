---
layout: default
title: Home
nav_order: 1
description: "A comprehensive IoT application framework for ESP32 microcontrollers with modern web-based configuration and control."
permalink: /
---

# Yarrboard Framework

**A comprehensive IoT application framework for ESP32 microcontrollers with modern web-based configuration and control.**

YarrboardFramework provides a complete embedded web server infrastructure for building ESP32-based IoT devices. Designed for reuse across multiple firmware projects, it delivers a standardized, production-ready-ish foundation with rich web interfaces, multi-protocol support, and home automation integration.

## License

**Mozilla Public License Version 2.0 (MPL-2.0)**

All framework source code is licensed under MPL-2.0, allowing commercial use and modification while requiring derivative works to share source code. Example code is released into the Public Domain.

## Features

### Core Capabilities

- **Web-Based Configuration**: Modern single-page application with real-time updates via WebSocket
- **JSON-Based Multi-Protocol**: Use the same JSON commands over Websocket, HTTP, Serial, or MQTT
- **Home Assistant Integration**: Automatic MQTT discovery protocol implementation
- **OTA Firmware Updates**: Development (Arduino OTA) and production (signed esp32FOTA) support
- **WiFi Management**: AP/Client modes with Improv provisioning for first-boot setup
- **Role-Based Authentication**: Three-tier access control (NOBODY, GUEST, ADMIN)
- **JSON-Based Configuration**: Human-readable configuration with web editor and backup/restore
- **Performance Monitoring**: IntervalTimer profiling, framerate tracking, and rolling statistics
- **Extensible Architecture**: Modular controller system with lifecycle hooks

### Protocol System

Dynamic JSON-based command protocol with:
- Registration of up to 50 commands (configurable via `YB_PROTOCOL_MAX_COMMANDS`)
- Role-based command access control
- Support for WebSocket, HTTP API, Serial, and MQTT interfaces
- Message rate limiting and statistics
- Lambda or member function callbacks
- Context information (communication mode, user role, client ID) passed to handlers

### Web Interface

- Bootstrap 5 responsive design with dark/light themes
- Real-time data updates via WebSocket
- Pages: Control, Status, Config, Settings, System
- Gzip-compressed assets embedded in firmware
- SHA256 ETag-based caching
- Offline-capable operation

---

[Next: Architecture →](docs/architecture.md)
