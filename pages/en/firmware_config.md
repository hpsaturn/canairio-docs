---
title: Firmware Configuration
tags:
  - firmware
  - device
  - debugging
  - config
keywords: firmware, devel, debugging, CLI, configuration, config, app
last_updated: "April 11, 2023"
summary: "CanAirIO App and CLI alternatives"
series: "firmware internals"
sidebar: english_sidebar
permalink: firmware_config.html
folder: en
---

## Overview

CanAirIO has **two ways** to configure its firmware. You able to use an Android device with our [CanAirIO App](https://canair.io/docs/app_usage) or with a serial terminal using the [CanAirIO CLI](https://canair.io/docs/cli).

Each alternative has some adventages and some features supported:

## Comparative

| Feature   |      App      |  CLI |
|:----------|:-------------:|:-----------:|
| Systems supported | Android 5+| Chrome, Edge and Serial terminal (Windows, Linux and Mac) |
| Technology | Bluetooth | Command line interface |
| Fixed station support | Yes | Yes |
| Mobile station support | Yes | No |
| WiFi config| Yes | Multiple networks, WiFi manager, SSID scanning |
| Home Assistant | Yes | Yes |
| Reboot device | Yes | Yes |
| Factory reset | Yes | Yes |
| Debug mode | Yes | Yes |
| CO2 Calibration | Yes | Coming soon |
| Fail safe mode | No | Yes |
| TX/RX custom pins | No | Yes |
| I2C custom pins | No | Coming soon |
| PAX enable | No | Yes |
| Emoji enable | No | Yes |
| Flip display | No | Yes |
| Miscellaneous | Yes | Yes |

{% include links.html %}
