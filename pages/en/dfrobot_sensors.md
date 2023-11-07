---
title: DFRobot Gravity Sensors
tags:
  - firmware
  - sensors
keywords: firmware, sensors, devices
last_updated: "November 12, 2023"
summary: "DFRobot Gravity Sensors"
series: "sensors"
sidebar: english_sidebar
permalink: dfrobot_sensors.html
folder: en
---

**NOTE:** At the moment, only the sensors mentioned below are included in CanAirIO firmware. Other sensors please submit a issue.

## Preparing the sensor for first use

In order to prevent address conflicts when using multiple sensors, we have prepared 8 groups with a total of 23 addresses. If necessary, You can use "change_sensor_iic_addr.ino" in the library file "example",to switch by modifying the group serial number configuration of "changeI2cAddrGroup()". After the serial port information displays "IIC addr change success!", power on again.

First of all, prepare the swiches depending on the sensor to be used.  
![](https://docutopia.sustrato.red/uploads/1d5a080f-9daa-449f-8d7d-54dc59f3444c.png)

SEL = 0
A1 = See the following table.
A2 = See the following table.

| A0| A1|I2C Address|Sensor asigned|
| -------- | -------- |------|------|
|0|0| 0x78|CO|
|0|1| 0x79|none|
|1|0|0x7A |NH3|
|1|1|0x7B |NO2|

The address group must be assigned, by software, to group 7.

## Why is it necessary to make this change?

As CanAirIO can use several sensor models, the i2c addresses can match. This is a problem, which creates a malfunction in the device.
For example, in sensor BME280, BMP280, BME680, they use addresses 0x77, 0x76 and 0x77, respectively. The standard address, with which the DFRobot gravity sensors are configured, is 0x77, if not changed, and used in combination with the BME680, a conflict occurs.

## Group changer sketch

Please load this sketch to change default group to 7, following this instructions:

***In platformio, create a new proyect, and place this code in main.cpp:***

```cpp
  /*
  * @file  changeSensorI2CAddr.ino
  * @brief The sensor proactively reports all the data, only i2c communication can use this demo.
  * @n Experimental mode: connect sensor communication pin to the main controller and burn
  * @n Communication mode select, DIP switch SEL: 0: I2C, 1: UART
  * @n Group serial number         address in the group
  * @n A0 A1 DIP level 00    01    10    11
  * @n 1            0x60  0x61  0x62  0x63
  * @n 2            0x64  0x65  0x66  0x67
  * @n 3            0x68  0x69  0x6A  0x6B
  * @n 4            0x6C  0x6D  0x6E  0x6F
  * @n 5            0x70  0x71  0x72  0x73
  * @n 6 (Default address group) 0x74  0x75  0x76  0x77 (Default address)
  * @n 7            0x78  0x79  0x7A  0x7B
  * @n 8            0x7C  0x7D  0x7E  0x7F
  * @n i2c address select, default to 0x77, A1 and A0 are grouped into 4 I2C addresses.
  * @n             | A0 | A1 |
  * @n             | 0  | 0  |    0x74
  * @n             | 0  | 1  |    0x75
  * @n             | 1  | 0  |    0x76
  * @n             | 1  | 1  |    0x77   default i2c address   
  * @n Experimental phenomenon: serial print all the data
  * @copyright   Copyright (c) 2010 DFRobot Co.Ltd (http://www.dfrobot.com)
  * @license     The MIT License (MIT)
  * @author      PengKaixing(kaixing.peng@dfrobot.com)
  * @version     V1.0
  * @date        2021-03-28
  * @url         https://github.com/DFRobot/DFRobot_MultiGasSensor
*/
#include "DFRobot_MultiGasSensor.h"

/*
  A0	A1	I2C Address	Sensor asigned
   0	0	  0x78	CO
   0	1	  0x79	none
   1	0	  0x7A	NH3
   1	1	  0x7B	NO2
   
   */

#define I2C_ADDRESS    0x7B //0x7B for NO2, 0x7A for NH3, 0x78 for CO

  DFRobot_GAS_I2C gas(&Wire ,I2C_ADDRESS);

uint8_t scan()
{
  Wire.begin();
  for(uint8_t address = 1; address < 127; address++ ) {
    Wire.beginTransmission(address);
    if (Wire.endTransmission() == 0)
      return address;
  }
  return 0xff;
}
void setup() {
  Serial.begin(115200);
  gas.setI2cAddr(scan());
  //Sensor init, init serial port or I2C, depending on the communication mode currently used
  while(!gas.begin())
  {
    Serial.println("NO Deivces !");
    delay(1000);
  }

  //Change i2c address group
  while(gas.changeI2cAddrGroup(7)==0)
  {
    Serial.println("IIC addr change fail!");
    
    
    delay(1000);
  }  
  Serial.println("IIC addr change success!");
}

void loop(){
}
```

***Platformio.ini example. Please, adapt it to your esp32 board.***

```python
; PlatformIO Project Configuration File
;
;   Build options: build flags, source filter
;   Upload options: custom upload port, speed and extra flags
;   Library options: dependencies, extra library storages
;   Advanced options: extra scripting
;
; Please visit documentation for the other options and examples
; https://docs.platformio.org/page/projectconf.html

[env:esp32doit-devkit-v1]
platform = espressif32
board = esp32doit-devkit-v1
monitor_speed = 115200
monitor_filters = time
framework = arduino
lib_deps = 
    https://github.com/DFRobot/DFRobot_MultiGasSensor.git
```

After run the sketch, by monitor serial, if all the proces run correctly, you can se the message: IIC addr change success!  

Now you can load the CanAirIO firmware.  

Oficial documentation from manufacturer, ban be found [here](https://wiki.dfrobot.com/SKU_SEN0465toSEN0476_Gravity_Gas_Sensor_Calibrated_I2C_UART#target_0)

## Features

Factory calibrated, accurate measurement
High sensitivity, low power consumption
Excellent stability and anti-interference
Use I2C mode only
Long service life(2 years)
Compatible with 3.3~5.5V main controllers
32 modifiable I2C addresses
Reverse connection protection
Temperature compensation
Threshold alarm

## Specifications

Detection Gas: CO, O2, NH3, H2S, NO2, HCL, H2, PH3, SO2, O3, CL2, HF(Need to change different probe)
Working Voltage: 3.3～5.5V DC
Working Current: <5mA
Output Signal: I2C
Detection error: ±10% of output value or ±5% of full scale (whichever is greater)
Working Temperature: -20～50℃
Working Humidity: 15～90%RH (non-condensing)
Storage Temperature: -20～50℃
Storage Humidity: 15～90%RH (non-condensing)
Lifespan: >2 years (in the air)
Adapter Plate Size: 37×32mm

**Board Overview**

![](https://docutopia.sustrato.red/uploads/81df9bd4-0ec8-41cd-86d0-685cee7a55bd.png)

**Smart Gas Sensor Terminal**

| Label | Name | Function |
| -------- | -------- | -------- |
|1|	D/T|	I2C data line SDA |
|2|	C/R|	I2C clock line SCL|
|3|	GND||Ground|
|4|	+|	Power supply + (3.3-5V compatible)|

## Connections

![](https://docutopia.sustrato.red/uploads/f4979ad9-2898-4ec3-83e7-4dfd31c682a3.png)

## Characteristic Parameters

|SKU|SEN0466|SEN0469|SEN0471|
| -------- | -------- | -------- |--------|
|**Type**|CO|NH3|NO2|
|**Detection Range**|(0-1000) ppm|(0-100) ppm|(0-20) ppm|
|**Resolution**|1ppm|1ppm|0.1ppm|
|**Response Time (T90)**|≤30S|≤150S|≤30S|

## Credits

Author: @roberbike  
Maintainers: @hpsaturn  
