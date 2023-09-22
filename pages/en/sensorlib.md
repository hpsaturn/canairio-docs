---
title: CanAirIO SensorsLib
tags:
  - firmware
  - device
  - hardware
keywords: firmware, hardware
last_updated: "July 5, 2021"
summary: "CanAirIO multi sensors library"
series: "hardware"
sidebar: english_sidebar
permalink: sensorlib.html
folder: en
---


[![PlatformIO](https://github.com/kike-canaries/canairio_sensorlib/workflows/PlatformIO/badge.svg)](https://github.com/kike-canaries/canairio_sensorlib/actions/) [![Build Status](https://travis-ci.com/kike-canaries/canairio_sensorlib.svg?branch=master)](https://travis-ci.com/kike-canaries/canairio_sensorlib.svg?branch=master) ![ViewCount](https://views.whatilearened.today/views/github/kike-canaries/canairio_sensorlib.svg) 

## Overview

Generic sensor manager, abstractions and bindings of multiple sensors [libraries](https://github.com/kike-canaries/canairio_sensorlib/blob/master/unified-lib-deps.ini): Honeywell, Plantower, Panasonic, Sensirion, etc. and CO2 sensors. Also it's handling others environment sensors. This library is for general purpose, but also is the sensors library base of [CanAirIO project](https://canair.io/docs).

For developers also you can check the complete library documentation [here](http://hpsaturn.com/canairio_sensorlib_doc/html/classSensors.html)

## Supported sensors

### PM sensors

| Sensor model  | UART  | I2C  | Detection mode | Status |  
|:----------------------- |:-----:|:-----:|:-------:|:----------:|
| Honeywell HPMA115S0 | Yes | --- | Auto | DEPRECATED |
| Panasonic SN-GCJA5L | Yes | Yes | Auto | STABLE |
| Plantower models    | Yes | --- | Auto | STABLE |
| Nova SDS011         | Yes | --- | Auto | STABLE |
| IKEA Vindriktning   | Yes | --- | Select | STABLE |
| Sensirion SPS30     | Yes | Yes | Select / Auto | STABLE |

NOTE:  
Panasonic via UART in ESP8266 maybe needs select in detection.  

### CO2 sensors

| Sensor model  | UART  | I2C | Detection mode | Status |  
|:----------------------- |:-----:|:-----:|:-------:|:----------:|
| Sensirion SCD30    | --- | Yes | Auto | STABLE |
| Sensirion SCD4x    | --- | Yes | Auto | TESTING |
| MHZ19      | Yes | --- | Select | STABLE |
| CM1106    | Yes | --- | Select | STABLE |
| SenseAir S8 | Yes | --- | Select | STABLE |

### Environmental sensors

| Sensor model  | Protocol  | Detection mode | Status |  
|:----------------------- |:-----:|:-------:|:----------:|
| AM2320      | i2c |  Auto | STABLE |
| SHT31       | i2c |  Auto | STABLE |
| AHT10       | i2c |  Auto | STABLE |
| BME280      | i2c |  Auto | STABLE |
| BMP280      | i2c |  Auto | STABLE |
| BME680      | i2c |  Auto | STABLE |
| DfRobot SEN0469 NH3  | i2c |  Auto | TESTING |
| DFRobot SEN0466 CO | i2c |  Auto | TESTING |
| Geiger CAJOE | GPIO | Select | TESTING |
| DHTxx       | TwoWire |  Select | DISABLED |

NOTE:  
DHT22 is supported but is not recommended. Please see the documentation.  

### Platforms supported

| Platform  | Variants  | Notes | Status |  
|:----------------------- |:-----:|:-------:|:----------:|
| ESP32  | WROVER* | ESP32Devkit and similar (recommended) | STABLE  |
| ESP32S3  | LilyGo TDisplay | In testing | STABLE |
| ESP32C3  | Devkit v3 | In testing | STABLE |
| ESP8266  | 12 |  D1MINI tested and similar (old) | STABLE |
| Atmelsam  | seeed_wio_terminal | Only works via i2c on left port | STABLE |
| Arduino | Atmel  | Some third party libraries fails | IN PROGRESS |

## Features

- Unified variables and getters for all sensors 
- Auto UART port selection (Hw, Sw, UART1, UART2, etc)
- Multiple i2c sensors and one UART sensor supported at the same time
- Two I2C channel supported (Wire and Wire1)
- Real time registry of sensor units (see multivariable)
- Get vendor names of all devices detected
- Preselected main stream UART pins from popular boards
- Auto config UART port for Plantower, Honeywell and Panasonic sensors
- Unified calibration trigger for all CO2 sensors
- Unified CO2 Altitude compensation
- Unified temperature offset for CO2 and environment sensors
- Add support for Kelvin and Fahrenheit on environment and CO2 sensors
- Public access to main objects of each library (full methods access)
- Get unit symbol and name and each sub-sensor
- Get the main group type: NONE, PM, CO2 and ENV.
- Basic debug mode support toggle in execution

Full list of all sub libraries supported [here](https://github.com/kike-canaries/canairio_sensorlib/blob/master/unified-lib-deps.ini)

## Quick implementation

```java
sensors.setOnDataCallBack(&onSensorDataOk);   // all data read callback
sensors.init();                               // start all sensors and
```

## Full implementation

You can review a full implementation on [CanAirIO project firmware](https://github.com/kike-canaries/canairio_firmware/blob/master/src/main.cpp), but a little brief is the next:

```java
/// sensors data callback
void onSensorDataOk() {
    Serial.print("PM2.5: " + String(sensors.getPM25()));
    Serial.print(" CO2: "  + String(sensors.getCO2()));
    Serial.print(" CO2H: " + String(sensors.getCO2humi()));
    Serial.print(" CO2T: " + String(sensors.getCO2temp()));
    Serial.print(" H: "    + String(sensors.getHumidity()));
    Serial.println(" T: "  + String(sensors.getTemperature()));
}

/// sensors error callback
void onSensorDataError(const char * msg){
    Serial.println("Sensor read error: "+String(msg));
}

void setup() {

    sensors.setOnDataCallBack(&onSensorDataOk);     // all data read callback
    sensors.setOnErrorCallBack(&onSensorDataError); // [optional] error callback
    sensors.setSampleTime(15);                      // [optional] sensors sample time (default 5s)
    sensors.setTempOffset(cfg.toffset);             // [optional] temperature compensation
    sensors.setCO2AltitudeOffset(cfg.altoffset);    // [optional] CO2 altitude compensation
    sensors.setSeaLevelPressure(1036.25);           // [optional] Set sea level pressure in hpa
    sensors.setDebugMode(false);                    // [optional] debug mode to get detailed msgs
    sensors.detectI2COnly(true);                    // [optional] force to only i2c sensors
    sensors.setTemperatureUnit(TEMPUNIT::KELVIN);   // comment it for Celsius or set Fahrenheit
    sensors.init();                                 // Auto detection to UART and i2c sensors

    // Alternatives only for UART sensors (TX/RX):

    // sensors.init(SENSORS::Auto);                 // Auto detection to UART sensors (Honeywell, Plantower, Panasonic)
    // sensors.init(SENSORS::SGCJA5);               // Force UART detection to Panasonic sensor
    // sensors.init(SENSORS::SSPS30);               // Force UART detection to Sensirion sensor
    // sensors.init(SENSORS::SMHZ19);               // Force UART detection to Mhz14 or Mhz19 CO2 sensor
    // sensors.init(SENSORS::SDS011);               // Force UART detection to SDS011 sensor
    // sensors.init(SENSORS::IKEAVK);               // Force UART detection to IKEA Vindriktning sensor
    // sensors.init(SENSORS::SCM1106);              // Force UART detection to CM1106 CO2 sensor
    // sensors.init(SENSORS::SAIRS8);               // Force UART detection to SenseAirS8 CO2 sensor
    // sensors.init(SENSORS::Auto,PMS_RX,PMS_TX);   // Auto detection on custom RX,TX
  
    // Also you can access to sub-library objects, and perform for example calls like next:

    // sensors.sps30.sleep()
    // sensors.bme.readPressure();
    // sensors.mhz19.getRange();
    // sensors.scd30.getTemperatureOffset();
    // sensors.aht10.readRawData();
    // sensors.s8.set_ABC_period(period)
    // ...

    delay(500);
}

void loop() {
    sensors.loop();  // read sensor data and showed it
}
```

## Multivariable demo

In this [demo](https://www.youtube.com/watch?v=-5Va47Bap48) on two different devices with multiple sensors, you can choose the possible sub sensors units or variables:

[![CanAirIO multivariable demo](https://img.youtube.com/vi/-5Va47Bap48/0.jpg)](https://www.youtube.com/watch?v=-5Va47Bap48)

In this [demo](https://www.youtube.com/watch?v=uxlmP905-FE) on a simple sketch you could have a dinamyc list of variables of multiple sensors brands:

[![CanAirIO Sensors Lib DEMO with M5CoreInk](https://img.youtube.com/vi/i15iEF47CbY/0.jpg)](https://youtu.be/i15iEF47CbY)

## Multivariable alternative implementation

The last version added new getters to have the current status of each unit of each sensor connected to the device in real time. Also you can retrieve the list of device names and other stuff:  

For example:

```cpp
#include <Arduino.h>
#include <Sensors.hpp>

void printSensorsDetected() {
    uint16_t sensors_count =  sensors.getSensorsRegisteredCount();
    uint16_t units_count   =  sensors.getUnitsRegisteredCount();
    Serial.println("-->[MAIN] Sensors detected count\t: " + String(sensors_count));
    Serial.println("-->[MAIN] Sensors units count  \t: "  + String(units_count));
    Serial.print(  "-->[MAIN] Sensors devices names\t: ");
    int i = 0;
    while (sensors.getSensorsRegistered()[i++] != 0) {
        Serial.print(sensors.getSensorName((SENSORS)sensors.getSensorsRegistered()[i - 1]));
        Serial.print(",");
    }
    Serial.println();
}

void printSensorsValues() {
    Serial.println("\n-->[MAIN] Preview sensor values:");
    UNIT unit = sensors.getNextUnit();
    while(unit != UNIT::NUNIT) {
        String uName = sensors.getUnitName(unit);
        float uValue = sensors.getUnitValue(unit);
        String uSymb = sensors.getUnitSymbol(unit);
        Serial.print("-->[MAIN] " + uName + ": " + String(uValue) + " " + uSymb);
        unit = sensors.getNextUnit();
    }
}

void onSensorDataOk() {
    Serial.println("======= E X A M P L E   T E S T =========");
    printSensorsDetected();
    printSensorsValues(); 
    Serial.println("=========================================");
}

/******************************************************************************
*  M A I N
******************************************************************************/

void setup() {
    Serial.begin(115200);
    delay(100);
    sensors.setSampleTime(5);                       // config sensors sample time interval
    sensors.setOnDataCallBack(&onSensorDataOk);     // all data read callback
    sensors.setDebugMode(true);                     // [optional] debug mode
    sensors.detectI2COnly(false);                   // disable force to only i2c sensors
    sensors.init();                                 // Auto detection to UART and i2c sensors
}

void loop() {
    sensors.loop();  // read sensor data and showed it
}
```

## UART detection demo

[![CanAirIO auto configuration demo](https://img.youtube.com/vi/hmukAmG5Eec/0.jpg)](https://www.youtube.com/watch?v=hmukAmG5Eec)

CanAirIO sensorlib auto configuration demo on [Youtube](https://www.youtube.com/watch?v=hmukAmG5Eec)

## Wiring

The current version of library supports 3 kinds of wiring connection, UART, i2c and TwoWire, in the main boards the library using the defaults pins of each board, but in some special cases the pins are:

### Predefined UART

The library has [pre-defined some UART pin configs](https://github.com/kike-canaries/canairio_sensorlib/blob/master/src/Sensors.hpp#L19-L52), these are selected on compiling time. Maybe you don't need change anything with your board, and maybe the nexts alternatives works for you:

| Board model    |  TX   | RX  |      Notes |
|:---------------|:---:|:---:|:------------------:|
| ESP32GENERIC   | 1  | 3  | ESP32 Pio defaults
| TTGOT7 / ESP32DEVKIT / D1MINI / NODEFINED | 16 | 17 | CanAirIO devices **
| TTGO_TDISPLAY  | 12 | 13 | |
| M5COREINK      | 14 | 13 | |
| TTGO TQ        | 18 | 13 | |
| HELTEC         | 18 | 17 | |
| WEMOSOLED      | 15 | 13 | |
| ESP32PICOD4    | 3  | 1  | |
  
** This pines are when you compile your project without specific any build variable or you board isn't in the list.  

### Custom UART

Also you could define a custom UART pins in the init() method and select specific sensors model, like this:

```cpp
sensors.init(SENSORS::SDS011,yourRX,yourTX); // custom RX, custom TX pines.
```

## I2C (recommended)

We are using the default pins for each board, some times it's pins are 21,22, please check your board schematic.

## Projects using this Library

- [CanAirIO Device](https://github.com/kike-canaries/canairio_firmware): ESP32 Air quality device for mobile and fixed stations. (PM2.5 and CO2)
- [Medidor de CO2](https://emariete.com/medidor-co2-display-tft-color-ttgo-t-display-sensirion-scd30): Un medidor de CO2 de alta calidad con pantalla en color. (CO2)  
- [M5CoreInk Multi Sensor](https://github.com/hpsaturn/co2_m5coreink#readme): Wall CO2, T, H, P, Alt, sensor with low consumption (30 days)

## Source code

For the last version of this documentation and the source code of the library, please enter [here](https://github.com/kike-canaries/canairio_sensorlib#readme)

---

## Credits

Thanks to all collaborators and [CanAirIO](https://canair.io) community for testing and reports. Visit us on [Telegram](https://t.me/canairio)

