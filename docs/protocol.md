---
layout: default
title: Protocol Reference
nav_order: 7
description: "JSON command protocol over WebSocket, HTTP, Serial, and MQTT transports."
---

# Protocol Reference

All communication with Yarrboard uses JSON messages over one of four transports: WebSocket, HTTP POST API, Serial, or MQTT. Every request must include a `cmd` field specifying the command to execute. An optional `msgid` field (unsigned integer) may be included in any request; if present, it will be echoed back in the response so clients can correlate replies to requests. Responses always include a `msg` field identifying the message type. Errors are returned as `{"msg": "status", "status": "error", "message": "..."}` and successes as `{"msg": "status", "status": "success", "message": "..."}`.

Commands are protected by one of three authorization roles: **nobody** (no authentication required), **guest**, and **admin**. Over WebSocket and Serial, clients can authenticate once using the `login` command (`{"cmd":"login","user":"...","pass":"..."}`) and remain authenticated for the session. Over the HTTP API and MQTT, credentials must be included with each request via `user` and `pass` fields.

Details of the protocol can be found in ```src/controllers/ProtocolController.cpp```

## Websockets Protocol

Yarrboard provides a websocket server on **http://example.local/ws**

JSON is sent and received as text.  Clients communicating over websockets can send a **login** command, or include your username and password with each request.

## Web API Protocol

Yarrboard provides a POST API endpoint at **http://example.local/api/endpoint**

Examples of how to communicate with this endpoint:

```
curl -i -d '{"cmd":"ping"}'  -H "Content-Type: application/json"  -X POST http://example.local/api/endpoint
curl -i -d '{"cmd":"set_channel","user":"admin","pass":"admin","id":0,"state":true}'  -H "Content-Type: application/json"  -X POST http://example.local/api/endpoint
```
Note, you will need to pass your username/password in each request with the Web API.

Additionally, there are a few convenience urls to get basic info.  These are GET only and optionally accept the **user** and **pass** parameters.

* http://example.local/api/config
* http://example.local/api/stats
* http://example.local/api/update

Some example code:

```
curl -i -X GET http://example.local/api/config
curl -i -X GET 'http://example.local/api/config?user=admin&pass=admin'
curl -i -X GET 'http://example.local/api/stats?user=admin&pass=admin'
curl -i -X GET 'http://example.local/api/update?user=admin&pass=admin'
```

## Serial API Protocol

Yarrboard provides a USB serial port that communicates at 115200 baud.  It uses the same JSON protocol as websockets and the web API.

Clients communicating over serial can send a **login** command, or include your username and password with each request.

Each command should end with a newline (\n) and each response will end with a newline (\n).

This port is also used for debugging, so make sure you check that each line parses into valid JSON before you try to accept it as an actual command.  It is possible that you will receive non-json input.  Simply ignore anything that does not parse into JSON and does not contain a `msg` field.

## MQTT API

If you have MQTT enabled, as well as the 'Protocol over MQTT' option enabled, you can send commands over MQTT to ```yarrboard/{hostname|uuid}/command```.  Response data will be written to ```yarrboard/{hostname|uuid}/response``` or you can pull data directly from MQTT topics.

---

[← Previous: Configuration](configuration.md) \| [Next: Security →](security.md)
