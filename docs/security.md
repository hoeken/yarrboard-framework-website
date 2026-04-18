---
layout: default
title: Security
nav_order: 7
description: "Role-based authentication, session management, and HTTPS support."
---

# Security

## Authentication System

Three-tier role-based access control:

| Role | Access Level |
|------|-------------|
| `NOBODY` | No access (not authenticated) |
| `GUEST` | Read-only access, safe commands |
| `ADMIN` | Full access, configuration changes, system control |

## Session Management

- Independent sessions for WebSocket, HTTP API, and Serial
- Cookie-based persistent login for HTTP
- Per-connection authentication state
- Configurable credentials via app configuration

## HTTPS Support

Optional SSL/TLS support:
- Custom certificate and private key
- Configurable via PsychicHttp
- Resource-intensive on ESP32 (limit concurrent connections)

---

[← Previous: Protocol Reference](protocol.md) \| [Next: OTA Updates →](ota.md)
