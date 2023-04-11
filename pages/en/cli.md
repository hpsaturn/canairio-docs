---
title: Firmware Configuration (CLI)
tags:
  - firmware
  - device
  - debugging
  - config
keywords: firmware, devel, debugging, CLI, configuration, config
last_updated: "April 11, 2023"
summary: "CanAirIO CLI"
series: "firmware internals"
sidebar: english_sidebar
permalink: cli.html
folder: en
---


## Overview

CanAirIO CLI (Command Line Interface) is an easy-to-use tool that allows you to configure your device through a console or web console. You don't need a mobile phone or any special operating system - all you need is a browser (such as Chrome or Edge) or a serial terminal interface.

## Launch the CLI

Using your browser please enter to our [CanAirIO Web installer](https://canair.io/installer) and following the next steps:

![Web installer logs](/docs/images/cli_steps.jpg)

### Video - OLED configuration sample

[![CanAirIO CLI](https://raw.githubusercontent.com/hpsaturn/canairio-docs/main/images/cli_steps_video.jpg)](https://youtu.be/a1faUtuPIlQ)

## CLI main commands

```bash
ESP32WifiCLI Usage:

setSSID "YOUR SSID" set the SSID into quotes
setPASW "YOUR PASW" set the password into quotes
connect             save and connect to the network
list                list all saved networks
select <number>     select the default AP (default: last saved)
mode <single/multi> connection mode. Multi AP is a little slow
scan                scan for available networks
status              print the current WiFi status
disconnect          disconnect from the network
delete "SSID"       remove saved network
help                print this help

CanAirIO Commands:

reboot              perform a soft ESP32 reboot
clear               factory settings reset. (needs confirmation)
debug <on/off>      to enable debug mode
stime <time>        set the sample time in seconds
spins <TX> <RX>     set the UART pins
stype <sensor_type> set the UART sensor type. Integer.
sgeoh <GeohashId>   set geohash id. Choose it here http://bit.ly/geohashe
kset <key> <value>  set preference key value (e.g on/off or 1/0 or text)
klist               list valid preference keys
info                get the device information
exit                exit of the initial setup mode
setup               type this to start the configuration

```

### Setup mode

This command show a brief of the current settings on the device like this:

```bash
Setup Mode. Main presets:

CanAirIO device id	: U33TTGOT7AA17A
Device factory id	: 5aa178
Sensor geohash id	: u33dc6s
WiFi current status	: connected
Sensor sample time 	: 240
UART sensor model 	: GENERIC
UART sensor TX pin	: 16
UART sensor RX pin	: 17
Current debug mode	: disabled

    KEYNAME 	DEFINED 	VALUE 

    ======= 	======= 	===== 
 wifiEnable 	custom  	true 
  paxEnable 	default 	 
  emoEnable 	default 	 
    i2conly 	custom  	true 
  altoffset 	default 	 
    toffset 	default 	 

Type "klist" for advanced settings
Type "help" for available commands details
Type "exit" for leave the safe mode
```

**NOTE:** Please keep in mind that you need set the WiFi and the Geohash (localization of your station) settings to have the station ready. These settings help to others to know the air quality in your zone. We have a cloud where you able to fetch all data via our API, also you able to have a widget and map visualization of your station [here](http://influxdb.canair.io:8000/d/xRQpZACWk/fixed-stations?orgId=1&refresh=5m)


### Safe mode

When the CanAirIO firmware starts up, there is a 10-second window during which we can write the `setup` command. This allows us to configure special settings before completing the boot process. If we write `exit`, the normal boot process will continue.

This setup is important because it allows us to check for any potential incompatibilities with our `TX/RX` pins, remove any problematic network connections, or fix any other settings that may be causing issues with the firmware. By addressing these issues during the setup process, we can ensure a smoother and more reliable boot process.

## CLI advanced commands

To show all variables possiblities, please run `klist`, for example:

```bash

    KEYNAME   DEFAULT  DEFINITION
    =======   =======  ==============================
 wifiEnable   true     turn on/off current WiFi network
  paxEnable   true     turn on/off PAX counter (WiFi sniffer)
  emoEnable   true     turn on/off Emoticons visualization (OLED)
    i2conly   false    turn on/off forced only i2c sensors (fast boot)
  altoffset   0        altitude offset to CO2 sensors
    toffset   0        temp offset (positive float will be a substraction)
debugEnable   false    turn on/off debug mode (verbose output)
flipVEnable   false    turn on/off flip vertical on OLED and TFT screens
homeaEnable   true     Home Assitant enable/disable	
anaireEnable  true     Anaire cloid enable/disable	
  ifxEnable   true     InfluxDb publication enable/disable
      ifxdb   canairio InfluxDb database
      ifxip   canairio IP address	(default: canairio server)
      ifxpt   8086     port
    hassusr   hassusr  Home Assistant username
    hasspsw   hasspsw  Home Assistant password
     hasspt   1883     Home Assistant port
   sealevel   1013.25  sea level value
fsafeEnable   true     fail safe enable/disable
wkrstEnable   false    wake up by reset button enable/disable
solarEnable   false    solar station mode enable/disable (experimental)
  deepSleep   0        solar station deep sleep time (experimental)
```

{% include links.html %}

