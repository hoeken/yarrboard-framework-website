---
layout: default
title: Debugging & Logging
nav_order: 12
description: "Multi-output logging, core dump support, and debug output features."
---

# Debugging and Logging

## YarrboardPrint System

Multi-output logging to multiple sinks simultaneously:
```cpp
#include <YarrboardPrint.h>

// Logs to Serial, USB CDC, and WebSocket clients
YBP.println("Debug message");

// Add custom print sink
YBP.addPrinter(&myPrintSink);
```

## Core Dump Support

Automatic detection and extraction of ESP32 core dumps:
- Detects crashes on boot
- Extracts core dump from flash
- Provides base64-encoded dump for analysis
- Clears dump after extraction

## Debug Output

- Startup logs captured and available via WebSocket
- Reset reason detection and reporting
- IntervalTimer profiling per controller
- WebSocket message rate statistics

---

[← Previous: Hardware & Performance](hardware.md) \| [Next: Contributing & Support →](contributing.md)
