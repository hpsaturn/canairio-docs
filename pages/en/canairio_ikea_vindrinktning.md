---
title: CanAirIO IKEA Vindriktning
tags:
  - firmware
  - device
  - hardware
  - hacks
keywords: firmware, hardware, IKEA, Vindriktning, device, hacks
last_updated: "August 31, 2022"
summary: "CanAirIO IKEA Vindriktning implementation"
series: "hardware"
sidebar: english_sidebar
permalink: canairio_ikea.html
folder: en
---

![photo_2022-08-31_21-53-42](https://user-images.githubusercontent.com/20878412/187770546-47b742a8-648c-437f-8efb-35cd598edbc3.jpg)

## Overview

CanAirIo now supports the Ikea Vindriktning sensor, via uart/serial port.
IKEA has released the air quality sensor Vindriktning which measures the PM2.5 density in the air, and shows the classification by a green, yellow or red LED.
Has inside a Cubic PM1006 particulate sensor.  

![photo_2022-08-31_21-54-38](https://user-images.githubusercontent.com/20878412/187770668-4f6a04bc-c6f0-4b3f-bcf7-5c38f7690cd7.jpg)

The micro controller that reads the sensor to control the LEDs, does so over an UART serial connection, and that, the Tx/Rx lines, were exposed via  a set of test pads, along with 5v and Ground power. This makes it easy to attach a second micro controller to the Rx and Tx lines, to read the response when the sensor is polled.

## Schematic

![pm1006](https://user-images.githubusercontent.com/20878412/187774297-f0ff122e-41a5-4bce-8f90-2ba7d0a8dd25.png)

## Components

* VINDRIKTNING air quality sensor with power adapter and usb-c cable.
* Esp32 board. TTGO T7 version 1.3
* Soldering iron with solder.
* Cables for soldering
* Screwdriver

## Assembling instructions

- Unscrew the case

- Strip the ends on 4 short pieces of wire.

- Solder the 4 leads to the test pads labelled 5v, GND, ISPDA and REST.

- Solder the 5V to 5V, G to G and REST to Rx2 and ISPDA to Tx2 (assuming using a TTGO T7 v1.3).  

![photo_2022-08-31_21-54-53](https://user-images.githubusercontent.com/20878412/187770734-2581912c-d89b-45a4-b7c4-d69bd26e6c91.jpg)

- TTGO T7 solder detail:  

![photo_2022-08-31_21-55-02](https://user-images.githubusercontent.com/20878412/187770770-5ba16449-87ce-4d6e-8b50-447d8c807457.jpg)

- Load the CanAirIo firmware, following this [instructions](https://canair.io/docs/firmware_upload.html#canairio-web-installer).

- Place the TTGO T7 in the empty space above the sensor.  

![photo_2022-08-31_21-54-33](https://user-images.githubusercontent.com/20878412/187770610-c0991767-62a2-40ce-acb8-0fca5cd25016.jpg)

- Screw the case back together

## Tested boards

| Board | Firmware |   |
| -------- | -------- | -------- |
| TTGO T7 v1.3     | Rev 918     |     |


## Disclaimer

For review our disclaimer, policy privacy and warranty of CanAirIO devices. please enter [here](https://canair.io/docs/disclaimer.html)

{% include links.html %}

