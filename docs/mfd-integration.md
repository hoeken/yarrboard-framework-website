---
layout: default
title: MFD Integration
nav_order: 11
description: "Automatic discovery and app interface with marine MFDs."
---

# Marine MFD Integration

Yarrboard was originally designed for electronics on a boat with navigation displays like B&G, Garmin, Raymarine, etc.  The main interface is a HTML5 web app, which can be loaded by any compatible display.

Currently, only Navico MFDs are supported (B&G, Simrad, etc.). Future support for other brands is possible, but I only have access to B&G.  Pull requests accepted.

## B&G Support

Simply enable MFD support and Yarrboard will broadcast the app information to all of your displays.  They will pick it up automatically and add an icon to your home screen.

B&G displays run an absolutely ancient version of Chrome, and occassionally it will crash.  If the app becomes non-responsive, you can go to **Gear Icon -> Features -> Manage apps -> Your App**. Then click **Stop**, **Uninstall**, and **Install**.  This should restart Chrome and the app will work again.

### Development Usage

To enable this functionality in your app, add this to your header:

```cpp
#include <controllers/NavicoController.h>

YarrboardApp yba;
NavicoController navico(yba);
```

Then register the controller in your main function:

```cpp
  yba.registerController(navico);
```

---

[← Previous: Home Assistant](home-assistant.md) \| [Next: Hardware & Performance →](hardware.md)
