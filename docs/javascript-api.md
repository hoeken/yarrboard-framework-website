---
layout: default
title: JavaScript API
nav_order: 12
description: "Frontend JavaScript API for building custom web interfaces with the YB namespace."
---

# Frontend JavaScript API

The YarrboardFramework provides a comprehensive JavaScript API for building custom web interfaces. The frontend code is organized into modules under the global `YB` namespace.

## Core Components

### YB.App - Application Core

The main application controller that manages the web interface lifecycle:

```javascript
// Initialize your custom code after the app starts
YB.App.onStart(function() {
  console.log("App is ready!");
});

// Register message handlers for server messages
YB.App.onMessage("custom_message", function(msg) {
  console.log("Received:", msg);
});

// Send commands to the server
YB.client.send({
  cmd: "my_command",
  param1: "value1"
});
```

**Key Methods:**
- `YB.App.onStart(callback)` - Register callback to run when app initializes
- `YB.App.onMessage(messageName, callback)` - Handle messages from the server
- `YB.App.addPage(page)` - Add a custom page to the interface
- `YB.App.removePage(name)` - Remove a page from the interface
- `YB.App.getPage(name)` - Get a page object by name
- `YB.App.openPage(name)` - Navigate to a specific page
- `YB.App.showAlert(message, type)` - Display an alert (types: 'danger', 'success', 'primary')

### YB.Page - Page Management

Create custom pages with navigation and lifecycle hooks:

```javascript
// Create a custom page
let myPage = new YB.Page({
  name: 'custom',                    // Unique identifier
  displayName: 'My Page',            // Display name in navbar
  permissionLevel: 'guest',          // Access level: 'nobody', 'guest', 'admin'
  showInNavbar: true,                // Show in navigation menu
  position: 'home',                  // Position relative to another page
  ready: true,                       // Page is ready to display immediately
  content: '<h1>Hello World</h1>'    // HTML content
});

// Add lifecycle hooks
myPage.onOpen(function() {
  console.log("Page opened");
  // Start polling, load data, etc.
});

myPage.onClose(function() {
  console.log("Page closed");
  // Stop polling, cleanup, etc.
});

// Add the page to the app
YB.App.addPage(myPage);
```

**Page Properties:**
- `name` - Unique page identifier
- `displayName` - Display name in navigation
- `permissionLevel` - Access control ('nobody', 'guest', 'admin')
- `showInNavbar` - Whether to show in navigation menu
- `position` - Position in navbar ('first', 'last', or another page name)
- `ready` - Whether page is ready to display
- `content` - HTML content of the page

**Page Methods:**
- `setContent(html)` - Update page content
- `onOpen(callback)` - Register callback when page opens
- `onClose(callback)` - Register callback when page closes

### YB.SettingsPanel - Settings Panel Management

Create custom settings panels that integrate seamlessly with the built-in Settings page:

```javascript
// Create a custom settings panel
let customPanel = new YB.SettingsPanel({
  name: 'custom',                    // Unique identifier
  displayName: 'Custom Settings',    // Display name in settings navbar
  position: 'general',               // Position relative to another panel
  useTitle: true,                    // Show displayName as heading (default: true)
  roundedBorder: true,               // Add border styling (default: true)
  content: '<p>Your settings here</p>' // HTML content
});

// Add lifecycle hooks
customPanel.onOpen(function() {
  console.log("Settings panel opened");
  // Load settings data, start polling, etc.
});

customPanel.onClose(function() {
  console.log("Settings panel closed");
  // Save settings, cleanup, etc.
});

// Add the panel to the settings page
YB.App.addSettingsPanel(customPanel);
```

**SettingsPanel Properties:**
- `name` - Unique panel identifier
- `displayName` - Display name in settings navigation
- `position` - Position in navbar ('first', 'last', or another panel name like 'wifi')
- `useTitle` - Whether to show displayName as a heading (default: true)
- `roundedBorder` - Whether to add border styling (default: true)
- `content` - HTML content of the panel

**SettingsPanel Methods:**
- `setContent(html)` - Update panel content
- `onOpen(callback)` - Register callback when panel opens
- `onClose(callback)` - Register callback when panel closes
- `show()` - Show the panel
- `hide()` - Hide the panel
- `remove()` - Completely remove the panel from the DOM

**App Settings Panel Methods:**
- `YB.App.addSettingsPanel(panel)` - Add a panel to the settings page
- `YB.App.getSettingsPanel(name)` - Get a panel object by name
- `YB.App.removeSettingsPanel(name)` - Remove a panel from the settings page
- `YB.App.openSettingsPanel(name)` - Navigate to and open a specific panel

**Complete Example:**

```javascript
YB.App.onStart(function() {
  // Create a custom settings panel
  let mySettingsPanel = new YB.SettingsPanel({
    name: 'mydevice',
    displayName: 'Device Settings',
    position: 'wifi',  // Insert after WiFi settings
    content: `
      <form id="mySettingsForm">
        <div class="mb-3">
          <label class="form-label">Device Name</label>
          <input type="text" class="form-control" id="deviceName">
        </div>
        <div class="mb-3">
          <label class="form-label">Update Interval (ms)</label>
          <input type="number" class="form-control" id="updateInterval">
        </div>
        <button type="submit" class="btn btn-primary">Save Settings</button>
      </form>
    `
  });

  // Load settings when panel opens
  mySettingsPanel.onOpen(function() {
    // Request current settings from server
    YB.client.send({ cmd: 'get_device_settings' });
  });

  // Handle form submission
  YB.App.onMessage('device_settings', function(msg) {
    $('#deviceName').val(msg.name);
    $('#updateInterval').val(msg.interval);
  });

  $('#mySettingsForm').submit(function(e) {
    e.preventDefault();
    YB.client.send({
      cmd: 'set_device_settings',
      name: $('#deviceName').val(),
      interval: parseInt($('#updateInterval').val())
    });
  });

  YB.App.addSettingsPanel(mySettingsPanel);
});
```

### YB.ChannelRegistry - Hardware Channel Management

Manage hardware channels (PWM, relays, sensors, etc.) dynamically:

```javascript
// Register a custom channel type
YB.ChannelRegistry.registerChannelType('mychannel', MyChannelClass);

// Get channels by type
let channels = YB.ChannelRegistry.getChannelsByType('pwm');

// Get specific channel by ID
let channel = YB.ChannelRegistry.getChannelById(1, 'pwm');

// Get channel by key
let channel = YB.ChannelRegistry.getChannelByKey('led1', 'pwm');
```

### YB.BaseChannel - Custom Channel Base Class

Create custom channel types by extending BaseChannel:

```javascript
function MyChannel() {
  YB.BaseChannel.call(this, 'mychannel', 'My Channel');
}
MyChannel.prototype = Object.create(YB.BaseChannel.prototype);

// Define configuration schema for validation
MyChannel.prototype.getConfigSchema = function() {
  return {
    id: { presence: true, numericality: { onlyInteger: true } },
    name: { presence: true, length: { maximum: 63 } },
    enabled: { presence: true },
    pin: { presence: true, numericality: { onlyInteger: true } }
  };
};

// Generate control UI for the home page
MyChannel.prototype.generateControlUI = function() {
  return `
    <div class="card">
      <div class="card-body">
        <h5>${this.name}</h5>
        <button id="btn-${this.id}" class="btn btn-primary">
          Toggle
        </button>
      </div>
    </div>
  `;
};

// Setup UI event handlers
MyChannel.prototype.setupControlUI = function() {
  $(`#btn-${this.id}`).click(() => {
    YB.client.send({
      cmd: 'toggle_channel',
      id: this.id
    });
  });
};

// Update UI with new data
MyChannel.prototype.updateControlUI = function() {
  let state = this.data.state ? 'ON' : 'OFF';
  $(`#btn-${this.id}`).text(state);
};

// Register the channel type
YB.ChannelRegistry.registerChannelType('mychannel', MyChannel);
```

### YB.Util - Utility Functions

Common utility functions for formatting and UI operations:

```javascript
// Format data
YB.Util.formatBytes(1024 * 1024)           // "1.0 MiB"
YB.Util.formatNumber(1234.567, 2)          // "1,234.57"
YB.Util.secondsToDhms(3665)                // "1 hour, 1 minute"
YB.Util.humanizeText("some_text")          // "Some Text"

// Form validation
YB.Util.showFormValidationResults(data, errors);
YB.Util.flashClass($('#myElement'), 'is-valid', 3000);

// Query parameters
let mode = YB.Util.getQueryVariable('mode');  // Get URL parameter

// Version comparison
if (YB.Util.compareVersions('1.2.0', '1.1.0')) {
  console.log("Newer version available");
}
```

### YB.log - Logging System

Logging functions for debugging:

```javascript
// Log messages (appear in console and debug terminal)
YB.log("Something happened");
YB.log(`Variable value: ${value}`);

// Setup debug terminal (done automatically)
YB.log.setupDebugTerminal();
```

## WebSocket Client (YB.client)

The YarrboardClient provides WebSocket communication:

```javascript
// Send commands to the server
YB.client.send({
  cmd: "get_status"
});

// Send with priority (bypasses queue)
YB.client.send({ cmd: "urgent" }, true);

// Client connection events are handled automatically
// But you can check connection status:
if (YB.client.isOpen()) {
  console.log("Connected to server");
}

// Get connection status
let status = YB.client.status();  // "CONNECTING", "CONNECTED", "RETRYING", "FAILED"
```

## Standard Message Types

The framework uses these standard message types:

| Message | Description | Trigger |
|---------|-------------|---------|
| `hello` | Initial connection handshake | On WebSocket connect |
| `config` | Board configuration data | On request, config changes |
| `update` | Real-time channel updates | Polled on home page |
| `stats` | System statistics | Polled on stats page |
| `status` | Status messages | Various operations |
| `error` | Error messages | Errors occur |
| `login` | Login response | Authentication |

## Complete Example

Here's a complete example showing how to customize the web interface:

```javascript
// Wait for app to start
YB.App.onStart(function() {

  // Update homepage with custom content
  let homePage = YB.App.getPage('home');
  homePage.setContent(`
    <h1>Welcome to My Device</h1>
    <p>Custom interface for my IoT project.</p>
    <button id="myButton" class="btn btn-primary">Do Something</button>
  `);

  // Setup button handler
  $('#myButton').click(function() {
    YB.client.send({
      cmd: 'custom_command',
      param: 'value'
    });
  });
});

// Create a custom page
let dataPage = new YB.Page({
  name: 'data',
  displayName: 'Data',
  permissionLevel: 'guest',
  showInNavbar: true,
  position: 'home',
  ready: false,
  content: '<div id="dataContent">Loading...</div>'
});

// When page opens, start polling for data
dataPage.onOpen(function() {
  this.pollInterval = setInterval(function() {
    YB.client.send({ cmd: 'get_data' });
  }, 1000);
});

// When page closes, stop polling
dataPage.onClose(function() {
  if (this.pollInterval) {
    clearInterval(this.pollInterval);
  }
});

YB.App.addPage(dataPage);

// Handle custom data messages
YB.App.onMessage('data_response', function(msg) {
  $('#dataContent').html(`
    <h3>Sensor Data</h3>
    <p>Temperature: ${msg.temperature}°C</p>
    <p>Humidity: ${msg.humidity}%</p>
  `);

  // Mark page as ready after first data
  let page = YB.App.getPage('data');
  if (page) page.ready = true;
});

// Handle custom commands from C++ side
YB.App.onMessage('custom_event', function(msg) {
  YB.App.showAlert(`Event: ${msg.description}`, 'info');
});

// Remove pages you don't need
YB.App.removePage('config');
```

## Integration with C++ Backend

The JavaScript communicates with the C++ backend using JSON messages:

**C++ Side:**
```cpp
// Register a command handler
yba.protocol.registerCommand(GUEST, "custom_command",
  [](JsonVariantConst input, JsonVariant output, ProtocolContext context) {
    String param = input["param"];

    // Send response back
    output["msg"] = "custom_event";
    output["description"] = "Command processed";
  }
);

// Broadcast message to all clients
JsonDocument doc;
doc["msg"] = "data_response";
doc["temperature"] = 25.5;
doc["humidity"] = 60.0;
yba.protocol.sendToAll(doc.as<JsonVariant>(), GUEST);
```

**JavaScript Side:**
```javascript
// Send command to C++
YB.client.send({
  cmd: "custom_command",
  param: "value"
});

// Receive response from C++
YB.App.onMessage("custom_event", function(msg) {
  console.log("Event:", msg.description);
});

YB.App.onMessage("data_response", function(msg) {
  console.log("Temperature:", msg.temperature);
  console.log("Humidity:", msg.humidity);
});
```

## Best Practices

1. **Use onStart callbacks** - Always initialize your code inside `YB.App.onStart()` to ensure the DOM is ready
2. **Clean up resources** - Use page `onClose()` callbacks to stop intervals, clear timeouts, etc.
3. **Handle ready states** - Set `page.ready = true` once your page has loaded its data
4. **Validate permissions** - Use appropriate `permissionLevel` for sensitive pages
5. **Use message handlers** - Register handlers with `YB.App.onMessage()` rather than polling
6. **Log for debugging** - Use `YB.log()` which appears in both console and the debug terminal
7. **Check connection** - Verify `YB.client.isOpen()` before sending commands in critical paths

## File Locations

When customizing the web interface, edit these files:

- `html/frontend.js` - Custom JavaScript code
- `html/index.html` - HTML structure (usually provided by framework)
- `html/style.css` - Custom CSS styles
- `html/logo.png` - Your project logo

The build system automatically processes these files and embeds them in the firmware.

---

[← Previous: Debugging & Logging](debugging.md) \| [Next: Contributing & Support →](contributing.md)
