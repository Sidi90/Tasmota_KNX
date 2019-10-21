## Sonoff-Tasmota KNX

<img src="/tools/logo/TASMOTA_FullLogo_Vector.svg" alt="Logo" align="right" height="90"/>

[Sonoff-Tasmota_KNX](https://github.com/ascillato/Sonoff-Tasmota_KNX) was a modification for [Sonoff-Tasmota](https://github.com/arendst/Sonoff-Tasmota) to add a basic functionality of the [KNX IP Protocol](https://www.knx.org/knx-en/index.php) (Multicast).
**Now it is integrated in [Sonoff-Tasmota](https://github.com/arendst/Sonoff-Tasmota)!**

[![GitHub version](https://img.shields.io/github/release/ascillato/Sonoff-Tasmota_KNX.svg)](https://github.com/ascillato/Sonoff-Tasmota_KNX/releases/latest) [![GitHub download](https://img.shields.io/github/downloads/ascillato/Sonoff-Tasmota_KNX/total.svg)](https://github.com/ascillato/Sonoff-Tasmota_KNX/releases/latest) [![License](https://img.shields.io/github/license/ascillato/Sonoff-Tasmota_KNX.svg)](https://github.com/ascillato/Sonoff-Tasmota_KNX/blob/development/LICENSE.txt) [![donate](https://img.shields.io/badge/donate-PayPal-yellow.svg)](https://www.paypal.me/ascillato)

This repository is for developing new KNX features for Sonoff-Tasmota.

If you like **Sonoff Tasmota KNX**, give it a star or fork it and contribute! [![GitHub stars](https://img.shields.io/github/stars/ascillato/Sonoff-Tasmota_KNX.svg?style=social&label=Star)](https://github.com/ascillato/Sonoff-Tasmota_KNX/stargazers)

Any help or comment is very welcome.

## Table of Contents

* [KNX Explanation](#knx-explanation)
* [Integration](#integration)
* [Requirements](#requirements)
* [Firmware](#firmware)
* [Usage Examples](#usage-examples)
* [Console Commands](#console-commands)
* [Development Road Map](#development-road-map)
* [Modifications to Sonoff-Tasmota](#modifications-to-sonoff-tasmota)
* [Contributors](#contributors)
* [Sonoff-Tasmota](#sonoff-tasmota)

## KNX Explanation ##

[<img src="https://www.knx.org/wGlobal/wGlobal/layout/images/knx-logo.png" />](https://www.knx.org/knx-en/knx/association/what-is-knx/index.php)

The [KNX IP Protocol](https://www.knx.org/knx-en/knx/association/what-is-knx/index.php) is an _international open standard_ for smart homes and smart buildings automation. It is a decentralized system. Each device can talk directly to each other without the need of a central controller or server. Any panel or server is just for telesupervision and for sending requests. KNX IP Protocol uses a UDP multicast on _224.0.23.12 : 3671_, so there is no need for a KNX Router unless you want to communicate to KNX Devices that are not in the WIFI Network (Twisted Pair, RF, Powerline).

Each device has a physical address (like a fixed IP) as **1 . 1 . 0** and that address is used for configuration purposes.

Each device can be configured with group addresses as **2 / 2 / 1** and that address can be used for sending/receiving commands.
So, for example, if 2 devices that are configured with the **2 / 2 / 1** for turning on/off their outputs, and other device send _Turn ON_ command to **2 / 2 / 1**, both devices will turn on their outputs.

## Integration ##

Several home automation systems have KNX support. For example, [Home Assistant](https://github.com/home-assistant/home-assistant) has a [XKNX Python Library](https://github.com/XKNX/xknx) to connect to KNX devices using a KNX Router. If you don't have a **KNX Router**, you can use a **Software KNX Router** like [KNXd](https://github.com/knxd/knxd) on the same Raspberry Pi than Home Assistant. KNXd is used by Home Assistant for reading this UDP Multicast, although KNXd has other cool features that need extra hardware like connect to KNX devices by Twister Pair, Power Line or RF.

If using the Home Assistant distribution called **Hassio**, everything for KNX is already included by default.

If you use the ETS (KNX Configurator Software) you can add any Sonoff Tasmota KNX as a dummy device.

## Requirements ##

All the libraries required for Sonoff-Tasmota are [here](https://github.com/ascillato/Sonoff-Tasmota_KNX/tree/development/lib) along with the extra required for the KNX Driver:

* [ESP KNX IP Library](https://github.com/ascillato/Sonoff-Tasmota_KNX/tree/development/lib/esp-knx-ip-0.5.2). A copy of this modified library is also available [here](https://github.com/ascillato/esp-knx-ip). The original is [here](https://github.com/envy/esp-knx-ip).

**Esp8266 board libraries:**
* v pre-2.6.x (stage) Works fine. No known issues. **RECOMMENDED**
* v2.5.2 Has some wifi issues. Sleep feature works fine but needs to be 0 for better KNX performance. Do not use.
* v2.5.1 Has some wifi and **security** issues. Do not use.
* v2.5.0 Has some wifi and **security** issues. Do not use.
* v2.4.2 Command sleep don't work in this version. It has **security** issues. Do not use.
* v2.4.1 Have some wifi and **security** issues. Do not use.
* v2.4.0 Have some wifi and **security** issues. Do not use.
* v2.3.0 It is slower than v2.4.2. Sleep feature works but it has **security** issues. Do not use.

## Firmware ##

In the [releases](https://github.com/ascillato/Sonoff-Tasmota_KNX/releases) section you can download the precompiled binaries for flashing ESP8266 devices.
If you need any other feature enabled or disabled, or a different Arduino core, you can use [TasmoCompiler](https://gitpod.io/#https://github.com/benzino77/tasmocompiler) ([readme](https://github.com/benzino77/tasmocompiler)) or you can modify the **my_user_config.h** file and build your firmware as explained in the [wiki](https://github.com/arendst/Sonoff-Tasmota/wiki).

## Implemented Features ##

The implemented features (up to now) in KNX for Tasmota are:

General:
* Buttons (just push)
* Relays (on/off/toggle)
* Lights (led strips, etc. but just on/off)

Sensor lists that you can use in KNX is (only one sensor per type):
* Temperature
* Humidity
* Energy (v, i, power)

Features that can be used with Tasmota's rules:
* Send KNX command (on/off)
* Receive KNX command (on/off)
* Send values by KNX (any float type, temperature for example)
* Receive values by KNX (any float type, temperature for example)
* Receive a KNX read request
* View/Set the Physical Address
* View/Set Group Address to send data
* View/Set Group Address to receive data

## Usage Examples ##

There are multiple possible configurations. Here are explained just a few as example. The options for selecting relays, buttons, sensors, etc. are only available if were configured on _Configure Module Menu_.

To configure KNX, enter on the Configuration Menu of Sonoff-Tasmota and select Configure KNX.

<img src="https://github.com/ascillato/Sonoff-Tasmota_KNX/blob/development/.github/Config_Menu.jpg" />
<img src="https://github.com/ascillato/Sonoff-Tasmota_KNX/blob/development/.github/KNX_menu.jpg" />

_Note on KNX communication enhancement option: As Wifi Multicast communication is not reliable in some wifi router due to IGMP problems or Snooping, an enhancement was implemented. This option increase the reliability by reducing the chances of losing telegrams, sending the same telegram 3 times. In practice it works really good and it is enough for normal home use. When this option is on, Tasmota will ignore toggle commands by KNX if those are sent more than 1 toggle per second. Just 1 toggle per second is working fine._

**1) Setting Several Sonoff to be controlled as one by a Home Automation System:**

We can set one of the group address to be the same in all the devices so as to turn them on or off at the same time.
In this case, so as to inform the status of all the relays to the Automation System, just one of the devices have to be configured as the responder. If you use the same Group Address for sending and receiving, you have to take into account not to make loops.

DEVICE 1

<img src="https://github.com/ascillato/Sonoff-Tasmota_KNX/blob/development/.github/1.jpg" />

DEVICE 2

<img src="https://github.com/ascillato/Sonoff-Tasmota_KNX/blob/development/.github/2.jpg" />

**2) Setting 2 Sonoff to be linked as stair lights:**

We can set one device to send the status of its output and another to read that and follow. And the second device can send the status of its button and the first device will toggle. With this configuration we can avoid to make a loop.

DEVICE 1

<img src="https://github.com/ascillato/Sonoff-Tasmota_KNX/blob/development/.github/3.jpg" />

DEVICE 2

<img src="https://github.com/ascillato/Sonoff-Tasmota_KNX/blob/development/.github/4.jpg" />

**3) Setting a button as initiator of a scene:**

Just setting one device to send the push of a button, and the rest just use that value to turn them on. In this case, there is no toggle. Every time the button is pushed, the turn on command is sent.

DEVICE 1

<img src="https://github.com/ascillato/Sonoff-Tasmota_KNX/blob/development/.github/5.jpg" />

DEVICE 2

<img src="https://github.com/ascillato/Sonoff-Tasmota_KNX/blob/development/.github/6.jpg" />

**4) Setting a Temperature sensor:**

We can configure to send the value of temperature or humidity every teleperiod. This teleperiod can be configured. See Sonoff Tasmota [wiki](https://github.com/arendst/Sonoff-Tasmota/wiki/Commands). It is recommended also to set the reply temperature address.

<img src="https://github.com/ascillato/Sonoff-Tasmota_KNX/blob/development/.github/7.jpg" />

**5) Using rules:**

More functionality can be added to Tasmota using rules.

* In the KNX Menu, can be set a Group Address to send data or commands by rules, as **KNX TX1** to **KNX TX5**

In rules we can use the command ``KnxTx_Cmnd1 1`` to send an ON state command to the group address set in **KNX TX1** slot of the KNX menu.
Also, we can use the command ``KnxTx_Val1 15`` to send a 15 value to the group address set in **KNX TX1** slot of the KNX menu.

* In the KNX Menu can be set a Group Address to receive commands by rules as **KNX RX1** to **KNX RX5**

In rules we can use the events to catch the reception of COMMANDS from KNX to those RX Slots.

Example: ``rule on event#knxrx_cmnd1 do var1 %value% endon`` to store the command received in the variable VAR1

In rules we can use the events to catch the reception of VALUES from KNX to those RX Slots.

Example: ``rule on event#knxrx_val1 do var1 %value% endon`` to store the value received in the variable VAR1

Also, if a Read request is received from KNX Network, we can use that in a rule as for example: ``rule on event#knxrx_req1 do knxtx_val1 %var3% endon``

## Console Commands ##

Command                | Payload | Description
-----------------------|---------|---------------------------------------------------------------------------
KnxTx_Cmnd\<x\>       | 0 / 1 | Send KNX Commands using the Group Address set on KNX Menu at KNX_TX\<x\> slot
KnxTx_Val\<x\>       | value | Send KNX float values using the Group Address set on KNX Menu at KNX_TX\<x\> slot
KNX_ENABLED            |       | View Status of KNX Communications
|                      | 0 / 1 | 0 - Set Disable to KNX Communications / 1 - Set Enable to KNX Communications
KNX_ENHANCED           |       | View Status of Enhanced mode for KNX Communications
|                      | 0 / 1 | 0 - Set to Disable / 1 - Set to Enable Enhanced KNX Communications Mode
KNX_PA                 |       | View the device KNX Physical Address (0.0.0 means not set)
|                      | \<x\>.\<x\>.\<x\> | Set the device KNX Physical Address (like 1.1.0)
KNX_GA                 |       | View the number of Group Address Configured to Send Data/Commands
|                      | \<x\>     | View the configuration for the Group Address number x Send Data/Commands
KNX_GA\<x\>            | \<y\>,\<z\>,\<z\>,\<z\> | Set the Group Address number \<x\> to Send Data/Commands
|                      |       | \<y\> is the parameter OPTION to send its status to the Group Address
|                      |       | \<z\>,\<z\>,\<z\> is the Group Address number to Send Data/Commands
KNX_CB                 |       | View the number of Group Address Configured to Receive Data/Commands
|                      | \<x\>     | View the configuration for the Group Address number x to Receive Data/Commands
KNX_CB\<x\>            | \<y\>,\<z\>,\<z\>,\<z\> | Set the Group Address number \<x\> to ReceiveData/Commands
|                      |       | \<y\> is the parameter OPTION to Receive its status from the Group Address
|                      |       | \<z\>,\<z\>,\<z\> is the Group Address number to Receive Data/Commands

Posible values for the parameter OPTION:

OPTION Value | Device Parameter
-----------------------|---------
1 | Relay 1
2 | Relay 2
3 | Relay 3
4 | Relay 4
5 | Relay 5
6 | Relay 6
7 | Relay 7
8 | Relay 8
9 | Button 1
10 | Button 2
11 | Button 3
12 | Button 4
13 | Button 5
14 | Button 6
15 | Button 7
16 | Button 8
17 | TEMPERATURE        
18 | HUMIDITY           
19 | ENERGY_VOLTAGE     
20 | ENERGY_CURRENT     
21 | ENERGY_POWER       
22 | ENERGY_POWERFACTOR 
23 | ENERGY_DAILY       
24 | ENERGY_START       
25 | ENERGY_TOTAL       
26 | KNX_SLOT1              
27 | KNX_SLOT2              
28 | KNX_SLOT3              
29 | KNX_SLOT4              
30 | KNX_SLOT5              
255 | EMPTY

## Development Road Map ##

**For Sonoff-Tasmota_KNX:**
- [x] Add Web Menu
- [x] Add Feature to Receive telegrams and modify Relay Status
- [x] Add Feature to Receive telegrams from multiple Group Addresses to modify just one relay status (useful for scenes)
- [x] Add Feature to Send telegrams of relay status change
- [x] Add Feature to Send telegrams of one relay status to multiple Group Addresses (useful for scenes)
- [x] Add Feature to Send telegrams of button pressed
- [x] Add Feature to receive telegrams to toggle relay status
- [x] Add Feature to read Temperature, Humidity from Tasmota
- [x] Add Feature to send Temperature, Humidity by a set interval (tasmota teleperiod)
- [x] Add Feature to receive command to read temperature, Humidity
- [x] Add Feature to recognize Tasmota config to show the same number of relays, buttons, etc.
- [x] Add Feature to Save Config
- [x] Add Feature to Load Config
- [x] Add Log Info
- [x] Complete all the language files with keys
- [x] Add support for other output devices supported by Tasmota
- [x] Add support for other sensors supported by Tasmota (TEMP, HUM, ENERGY)
- [x] Add commands for rules to send values and commands by KNX
- [x] Add commands for rules to set KNX Configurations
- [x] Add events for rules when receiving data from KNX and read requests
- [x] Add option for increase communication reliability (re send telegrams)
- [ ] Add option for multicast forced reconnection (needed for some routers that have IGMP conflict with actual esp8266 lib v2.3.0 to v2.5.0, and lwIP v1.4 to v2.1 - Send a telegram to itself. If it is received, multicast is ok, if not, reconnect)
- [ ] Add option to support KNX Snooping to debug KNX Network
- [ ] Add option for KNXnet/IP Tunneling
- [ ] Add option to repeat all KNX multicast broadcast (Tasmota to Tasmota communications) to KNXnet/IP Tunneling
- [ ] Add option to support ETS Programming
- [ ] Optimize code to reduce Flash and RAM

## Modifications to Sonoff-Tasmota ##

* Added the file _/sonoff/xdrv_11_KNX.ino_
* Added the entries `#define USE_KNX` and `#define USE_KNX_WEB_MENU` on _/sonoff/my_user_config.h_
* Added entries to the file _/sonoff/xdrv_01_webserver.ino_
* Added entries to the file _/sonoff/sonoff.ino_
* Added entries to the file _/sonoff/sonoff.h_
* Added entries to the file _/sonoff/settings.h_
* Added entries to the file _/sonoff/support.ino_
* Added entries to sensor files
* Added entries to language files

Up to now, enabling KNX uses +9.4 KB of code and +3.7 KB of memory for Tasmota. If it is enabled also the KNX Web Menu, it adds +8.3 KB more of code and +144 Bytes more of memory.

There is **NO CONFLICT** with MQTT, Home Assistant, Web, etc. Tests show fast response of all features running at same time.

## Contributors ##

* [ascillato](https://github.com/ascillato) ( Adrian Scillato )
* [sisamiwe](https://github.com/sisamiwe) - Thanks for the guide on using KNX and software testing and support
* [envy](https://github.com/envy) ( Nico Weichbrodt ) - Thanks for the patience and help with the modifications to ESP_KNX_IP.
* [arendst](https://github.com/arendst) ( Theo Arends ) - Thanks for the guide on Tasmota and for the ideas.
* [johannesbonn](https://github.com/johannesbonn) - Thanks for the patience on bug resolutions
* [RocketSience](https://github.com/RocketSience) - Thanks for the patience on bug resolutions
* [jeylites](https://github.com/jeylites) - Thanks for the patience on bug resolutions
* [Winni66](https://github.com/Winni66) - Thanks for the patience on bug resolutions
* [misc2000](https://github.com/misc2000) - Thanks for the testing on bug resolutions
* [mizrachiran](https://github.com/mizrachiran) ( Ran Mizrachi ) - Thanks for the testing on bug resolutions
* [smurfix](https://github.com/smurfix) ( Matthias Urlichs ) - Thanks for the KNX guiding and [KNXd](https://github.com/knxd/knxd) use.
* And many others providing testing, bug reporting and feature requests.

-----------------------------------------------------------------------------------------------------------------------------------

<img src="/tools/logo/TASMOTA_FullLogo_Vector.svg" alt="Logo" align="right" height="76"/>

# Sonoff-Tasmota
Alternative firmware for _ESP8266 based devices_ like [iTead](https://www.itead.cc/) _**Sonoff**_ with **web UI, rules and timers, OTA updates, custom device templates and sensor support**. Allows control over **MQTT**, **HTTP**, **Serial** and **KNX** for integrations with smart home systems. Written for Arduino IDE and PlatformIO.

[![GitHub version](https://img.shields.io/github/release/arendst/Sonoff-Tasmota.svg)](https://github.com/arendst/Sonoff-Tasmota/releases/latest)
[![GitHub download](https://img.shields.io/github/downloads/arendst/Sonoff-Tasmota/total.svg)](https://github.com/arendst/Sonoff-Tasmota/releases/latest)
[![License](https://img.shields.io/github/license/arendst/Sonoff-Tasmota.svg)](https://github.com/arendst/Sonoff-Tasmota/blob/development/LICENSE.txt)
[![Chat](https://img.shields.io/discord/479389167382691863.svg)](https://discord.gg/Ks2Kzd4)

If you like **Sonoff-Tasmota**, give it a star, or fork it, and contribute!

[![GitHub stars](https://img.shields.io/github/stars/arendst/Sonoff-Tasmota.svg?style=social&label=Star)](https://github.com/arendst/Sonoff-Tasmota/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/arendst/Sonoff-Tasmota.svg?style=social&label=Fork)](https://github.com/arendst/Sonoff-Tasmota/network)
[![donate](https://img.shields.io/badge/donate-PayPal-blue.svg)](https://paypal.me/tasmota)

See [RELEASENOTES.md](https://github.com/arendst/Sonoff-Tasmota/blob/development/RELEASENOTES.md) for release information.

In addition to the [release webpage](https://github.com/arendst/Sonoff-Tasmota/releases/latest) the binaries can also be downloaded from http://thehackbox.org/tasmota/release/

## Development
[![Dev Version](https://img.shields.io/badge/development%20version-v6.6.0.x-blue.svg)](https://github.com/arendst/Sonoff-Tasmota)
[![Download Dev](https://img.shields.io/badge/download-development-yellow.svg)](http://thehackbox.org/tasmota/)
[![Build Status](https://img.shields.io/travis/arendst/Sonoff-Tasmota.svg)](https://travis-ci.org/arendst/Sonoff-Tasmota)

See [sonoff/_changelog.ino](https://github.com/arendst/Sonoff-Tasmota/blob/development/sonoff/_changelog.ino) for detailed change information.

Unless your Tasmota powered device exhibits a problem or you need to make use of a feature that is not available in the Tasmota version currently installed on your device, leave your device alone - it works so don't make unnecessary changes! If the release version (i.e., the master branch) exhibits unexpected behaviour for your device and configuration, you should upgrade to the latest development version instead to see if your problem is resolved as some bugs in previous releases or development builds may already have been resolved.  

The Tasmota development codebase is checked every 1-2 hours for changes. If new commits have been merged and they compile successfuly, new binary files for every variant (excluding non-English languages) will be posted at http://thehackbox.org/tasmota/ (this web address can be used for OTA updates too). The last compiled commit number is also indicated on the same page. It is important to note that these binaries are based on the current development codebase. These commits are tested as much as is possible and are typically quite stable. However, it is infeasible to test on the hundreds of different types of devices with all the available configuration options permitted.  

Note that there is a chance, as with any upgrade, that the device may not function as expected. You must always account for the possibility that you may need to flash the device via the serial programming interface if the OTA upgrade fails. Even with the master release, you should always attempt to test the device or a similar prototype before upgrading a device which is in production or is hard to reach. And, as always, make a backup of the device configuration before beginning any firmware update. 

## Disclaimer
:warning: **DANGER OF ELECTROCUTION** :warning:

An ESP82xx Wi-Fi device is not a toy. It uses Mains AC so there is a danger of electrocution if not installed properly. If you don't know how to install it, please call an electrician. Remember: _**SAFETY FIRST**_. It is not worth risk to yourself, your family, and your home if you don't know exactly what you are doing. Never try to flash a device using the serial programming interface while it is connected to MAINS AC.

We don't take any responsibility nor liability for using this software nor for the installation or any tips, advice, videos, etc. given by any member of this site or any related site.

## Note
Please do not ask to add new devices unless it requires additional code for new features. If the device is not listed as a module, try using [Templates](https://github.com/arendst/Sonoff-Tasmota/wiki/Templates) first. If it is not listed in the [Tasmota Device Templates Repository](http://blakadder.github.io/templates) create your own [Template](https://github.com/arendst/Sonoff-Tasmota/wiki/Templates#creating-your-template-).

## Quick Install
Download one of the released binaries from https://github.com/arendst/Sonoff-Tasmota/releases and flash it to your hardware as [documented in the wiki](https://github.com/arendst/Sonoff-Tasmota/wiki/Flashing).

## Important User Compilation Information
If you want to compile Sonoff-Tasmota yourself keep in mind the following:

- Only Flash Mode **DOUT** is supported. Do not use Flash Mode DIO / QIO / QOUT as it might seem to brick your device. See [Wiki](https://github.com/arendst/Sonoff-Tasmota/wiki/Flashing) for background information.
- Sonoff-Tasmota uses a 1M linker script WITHOUT spiffs **1M (no SPIFFS)** for optimal code space. If you compile using ESP/Arduino library 2.3.0 then download the provided new linker script to your Arduino IDE or Platformio base folder. Later versions of ESP/Arduino library already contain the correct linker script. See [Wiki > Prerequisites](https://github.com/arendst/Sonoff-Tasmota/wiki/Prerequisites).
- To make compile time changes to Sonoff-Tasmota it can use the ``user_config_override.h`` file. It assures keeping your settings when you download and compile a new version. To use ``user_config.override.h`` you will have to make a copy of the provided ``user_config_override_sample.h`` file and add your setting overrides. To enable the override file you will need to use a compile define as documented in the ``user_config_override_sample.h`` file.

## Configuration Information
Please refer to the Installation and configuration articles in the [wiki](https://github.com/arendst/Sonoff-Tasmota/wiki).

## Migration Information
See [wiki migration path](https://github.com/arendst/Sonoff-Tasmota/wiki/Upgrade#migration-path) for instructions how to migrate to a major version. Pay attention to the following version breaks due to dynamic settings updates:

1. Migrate to **Sonoff-Tasmota 3.9.x**
2. Migrate to **Sonoff-Tasmota 4.x**
3. Migrate to **Sonoff-Tasmota 5.14**
4. Migrate to **Sonoff-Tasmota 6.x**

## Support Information
<img src="https://github.com/arendst/arendst.github.io/blob/master/media/sonoffbasic.jpg" width="250" align="right" />

For a database of supported devices see [Tasmota Device Templates Repository](https://blakadder.github.io/templates)

See [Wiki](https://github.com/arendst/Sonoff-Tasmota/wiki) for use instructions and how-to's.<br />
See [Community](https://groups.google.com/d/forum/sonoffusers) for forum.<br />
Visit [Discord Chat](https://discord.gg/Ks2Kzd4) for discussions and troubleshooting.

## Contribute
You can contribute to Sonoff-Tasmota by
- providing Pull Requests (Features, Proof of Concepts, Language files or Fixes)
- testing new released features and report issues
- donating to acquire hardware for testing and implementing or out of gratitude
- contributing missing documentation for features and devices on our [Wiki](https://github.com/arendst/Sonoff-Tasmota/wiki)

[![donate](https://img.shields.io/badge/donate-PayPal-blue.svg)](https://paypal.me/tasmota)

## Credits

### Libraries Used
Libraries used with Sonoff-Tasmota are:
- [ESP8266 core for Arduino](https://github.com/esp8266/Arduino)
- [Adafruit CCS811](https://github.com/adafruit/Adafruit_CCS811)
- [Adafruit ILI9341](https://github.com/adafruit/Adafruit_ILI9341)
- [Adafruit LED Backpack](https://github.com/adafruit/Adafruit-LED-Backpack-Library)
- [Adafruit SGP30](https://github.com/adafruit/Adafruit_SGP30)
- [Adafruit SSD1306](https://github.com/adafruit/Adafruit_SSD1306)
- [Adafruit GFX](https://github.com/adafruit/Adafruit-GFX-Library)
- [ArduinoJson](https://arduinojson.org/)
- [Bosch BME680](https://github.com/BoschSensortec/BME680_driver)
- [C2 Programmer](http://app.cear.ufpb.br/~lucas.hartmann/tag/efm8bb1/)
- [esp-epaper-29-ws-20171230-gemu](https://github.com/gemu2015/Sonoff-Tasmota/tree/displays/lib)
- [esp-knx-ip](https://github.com/envy/esp-knx-ip)
- FrogmoreScd30
- [I2Cdevlib](https://github.com/jrowberg/i2cdevlib)
- [IRremoteEsp8266](https://github.com/markszabo/IRremoteESP8266)
- [JobaTsl2561](https://github.com/joba-1/Joba_Tsl2561)
- [LinkedList](https://github.com/ivanseidel/LinkedList)
- [Liquid Cristal](https://github.com/marcoschwartz/LiquidCrystal_I2C)
- [MultiChannelGasSensor](http://wiki.seeedstudio.com/Grove-Multichannel_Gas_Sensor/)
- [NeoPixelBus](https://github.com/Makuna/NeoPixelBus)
- [NewPing](https://bitbucket.org/teckel12/arduino-new-ping/wiki/Home)
- [OneWire](https://github.com/PaulStoffregen/OneWire)
- [PubSubClient](https://github.com/knolleary/pubsubclient)
- [rc-switch](https://github.com/sui77/rc-switch)

### People inspiring me
People helping to keep the show on the road:
- David Lang providing initial issue resolution and code optimizations
- Heiko Krupp for his IRSend, HTU21, SI70xx and Wemo/Hue emulation drivers
- Wiktor Schmidt for Travis CI implementation
- Thom Dietrich for PlatformIO optimizations
- Marinus van den Broek for his EspEasy groundwork
- Pete Ba for more user friendly energy monitor calibration
- Lobradov providing compile optimization tips
- Flexiti for his initial timer implementation
- reloxx13 for his [TasmoAdmin](https://github.com/reloxx13/TasmoAdmin) management tool
- Joachim Banzhaf for his TSL2561 library and driver
- Gijs Noorlander for his MHZ19, SenseAir and updated PubSubClient drivers
- Emontnemery for his HomeAssistant Discovery concept and many code tuning tips
- Aidan Mountford for his HSB support
- Daniel Ztolnai for his Serial Bridge implementation
- Gerhard Mutz for multiple sensor & display drivers, Sunrise/Sunset, and scripting
- Nuno Ferreira for his HC-SR04 driver
- Adrian Scillato for his (security)fixes and implementing and maintaining KNX
- Gennaro Tortone for implementing and maintaining Eastron drivers
- Raymond Mouthaan for managing Wemos Wiki information
- Norbert Richter for his decode-config.py tool
- Andre Thomas for providing [thehackbox](http://thehackbox.org/tasmota/) OTA support and daily development builds
- Joel Stein and digiblur for their Tuya research and driver
- Frogmore42 and Jason2866 for providing many issue answers
- Blakadder for editing the wiki and providing template management
- Stephan Hadinger for refactoring light driver and enhancing HueEmulation
- tmo for designing the official logo
- Many more providing Tips, Wips, Pocs or PRs

## License

This program is licensed under GPL-3.0
