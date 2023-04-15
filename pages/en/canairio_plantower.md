---
title: CanAirIO Plantower PMS7003/G7
tags:
  - firmware
  - device
  - hardware
keywords: device, fixed stations, sensors, hardware, plantower
last_updated: "April 15, 2023"
summary: "CanAirIO Plantower PMS7003/G7 device supports"
series: "hardware"
sidebar: english_sidebar
permalink: canairio_plantower.html
folder: en
---

## Overview

The objective of this guide is to provide an easy-to-follow description of the process to construct a device for measuring air quality, temperature, and humidity using the **CanAirIO firmware**. It will provide a basic guide on how to connect each of the sensors to the development board, as well as how to load and configure the **CanAirIO firmware**. The focus will be on practicality and clarity, avoiding technical details to ensure that the process can be easily understood and replicated by anyone.

This guide covers the construction of a device that utilizes the **Plantower PMS7003/G7** sensor for measuring **PM2.5** particulate matter, the **SHT31** sensor for measuring temperature and humidity, and the **TTGO T-Display** development board, which features an **ESP32** chip and a 1.14-inch display.

## Components

| Part Ref             | Quantaty | Shop |
|----------------------|----------|------|
| TTGO T-Display       | 1        |   [Aliexpress](https://www.aliexpress.com/item/33048962331.html)   |
| Plantower PMS7003/G7 | 1        |   [Aliexpress](https://www.aliexpress.com/item/32623909733.html)   |
| SHT31                | 1        |   [Aliexpress](https://www.aliexpress.com/item/1005005072873580.html)   |

### TTGO T-Display pinout diagram

![TTGO T-Display](images/ttgo_tdisplay_pin_diagram.jpg)

### SHT31 pinout diagram

![SHT31](images/sht31_pin_diagram.jpg)

### Plantower PMS7003/G7 pinout diagram

![Plantower PMS7003](images/plantower_pin_diagram.jpg)

![Plantower PMS7003/G7 pinout](images/plantowerg7_pin_diagram.jpg)

![Plantower PMS7003/G7](images/plantower_PMS7003_G7.jpg)

## Schematics

![CanAirIO Plantower Schematics](images/canairio_plantower_schematics.jpg)

Please note that it is a general diagram, for details of pin connections please review the datasheet and pinout diagram of each component.

## Basic connection

You need to connect the **Plantower PMS7003/G7** sensor and the **SHT31** sensor to the **TTGO T-Display** development board. You should solder the cables and connect each cable to its corresponding pin as shown in the previous schematic.

You'll also need a cellphone charger with a USB-C cable to provide power to the TTGO T-Display development board. Once all the components have been connected correctly, you can use a plastic box to protect all the components as shown in the following image.

[![CanAirIO Plantower Making of](images/canairio_plantower_basic.jpg)](https://youtu.be/uUhUD58pEAQ)

## Firmware upload

We already have a super easy way for **install CanAirIO** firmware on any compatible **Arduino ESP32 board** like a TTGO T-Display board, in a few seconds:

![video_2021-11-13_23-36-10](https://user-images.githubusercontent.com/423856/141661066-0fafcaa9-98b4-419b-b1e7-4371f3cb99b8.gif)  

More info in [canair.io/installer](https://canair.io/installer.html)

Other alternatives for upload the CanAirIO firmware, [here](https://canair.io/docs/firmware_upload.html).

## Firmware Configuration (CLI)

**CanAirIO CLI** (Command Line Interface) is an easy-to-use tool that allows you to configure your device through a console or web console. You don’t need a mobile phone or any special operating system - all you need is a browser (such as Chrome or Edge) or a serial terminal interface.

More info in [canair.io/cli](https://canair.io/docs/cli.html)

{% include links.html %}
