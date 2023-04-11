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
setPASW "YOUR PASW"	set the password into quotes
connect             save and connect to the network
list                list all saved networks
select <number>   	select the default AP (default: last saved)
mode <single/multi>	connection mode. Multi AP is a little slow
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
kset <key> <value>	set preference key value (e.g on/off or 1/0 or text)
klist               list valid preference keys
info                get the device information
exit                exit of the initial setup mode
setup               type this to start the configuration

```

### Safe mode

When the CanAirIO firmware starts up, there is a 10-second window during which we can write the `setup` command. This allows us to configure special settings before completing the boot process. If we write `exit`, the normal boot process will continue.

This setup is important because it allows us to check for any potential incompatibilities with our `TX/RX` pins, remove any problematic network connections, or fix any other settings that may be causing issues with the firmware. By addressing these issues during the setup process, we can ensure a smoother and more reliable boot process.

## CLI advanced commands

To show all variables possiblities, please run `klist`, for example:

```bash
    KEYNAME 	DEFINED 	VALUE 

    ======= 	======= 	===== 
 wifiEnable 	custom  	true   
  paxEnable 	default 	 
  emoEnable 	default 	 
    i2conly 	custom  	true 
  altoffset 	default 	 
    toffset 	default 	 
debugEnable 	custom  	false 
flipVEnable 	default 	 
homeaEnable 	default 	 
anaireEnable 	default 	 
  ifxEnable 	custom  	true 
      ifxdb 	default 	 
      ifxip 	default 	 
      ifxpt 	default 	 
    hassusr 	default 	 
    hasspsw 	default 	 
     hasspt 	default 	 
   sealevel 	default 	 
fsafeEnable 	default 	 
wkrstEnable 	default 	 
solarEnable 	default 	 
  deepSleep 	default 	 
```

{% include links.html %}

