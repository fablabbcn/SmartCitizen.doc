Data Board
==========

The data board is a data-logger at the core of the sensors architecture supporting the Smart Citizen Kit and the Smart Citizen Station. This module is powered by an ARM M0+ 32-bits 48Mhz **SAMD21** running the Smart Citizen Firmware( (/Components/Firmware)), combining the low power consumption of the ARM M0 family with the power of a 32-bits processor with 32KB of RAM and 256KB of FLASH memory. This solution offers enough program storage and memory space to support multiple auxiliary sensors. This chip is used by the Arduino Zero and MKR boards, therefore benefiting from the open community built around these boards in particular and the Arduino project in general.

![](https://i.imgur.com/V3Uce0x.jpg)

<a class="github-button" data-size="large" href="https://github.com/fablabbcn/smartcitizen-kit-20" aria-label="Check the source code">Check the source code</a>

The data board also includes a Wi-Fi module, a micro SD card slot, an internal Flash and a battery management solution. In addition, it includes 4MB of extra Flash Memory for offline data storage, in case of network brownouts. The Wi-Fi Module is the well-known [Espressif ESP8266](https://www.espressif.com/en/products/hardware/esp8266ex/overview) IEEE 802.11 b/g/n Wi-Fi with 4MB Internal Flash for web content storage:

![](https://i.imgur.com/wYoz4G8.png)

The Data Board connects to the sensor board providing power, analog and digital communications (12 bits ADC, GPIO, I2C, I2S, VCC). The data board also includes a Seeed Studio standard Grove connector where off-the-shelf modules from the same manufacturer can be connected. The connector supports an independent I2C bus by default, but by software it can be configured to support other uses (GPIO, I2C and UART). It can supply power up to 750mA, and it can be enabled or disabled by software to save power.

The board includes a power unit, with a battery management system, capable of handling a variety of Lithium polymer cells. The batteries are connected to a standard JST-2 pin battery connector. The Smart Citizen Kit by default uses a 2000mAh battery, but larger capacities can be used. Under normal conditions, and depending on the sensors enabled, a 2000mAh battery can last between 24 hours (with all sensors enabled, and a 1-minute recording frequency) to more than a week. The board also features a _sleep mode_, through which drastically lower average consumption are achieved.

The controller allows the batteries to be easily charged using the boards micro USB connector using any standard USB power adapter like the ones used on Smartphones. On remote areas, it can also be powered using a selection of PV Panels like [Voltaics Systems](https://www.voltaicsystems.com/) 6W panel.

!!! tips "Light color codes"
	* **<font color="#FFF" style="BACKGROUND-COLOR: #FF0015; padding: 4px;">Red soft pulsing</font>** >> Apmode
	* **<font color="#FFF" style="BACKGROUND-COLOR: #0099FF; padding: 4px;">Blue soft pulsing</font>** >> wifi.
	* **<font color="#FFF" style="BACKGROUND-COLOR: #FF77EF; padding: 4px;">Pink soft pulsing</font>** >> sdcard.
	* **Other color + <font color="#FFF" style="BACKGROUND-COLOR: #FF9100; padding: 4px;">Orange soft pulsing</font>** >> on battery.
	* **Other color + <font color="#FFF" style="BACKGROUND-COLOR: #47D847; padding: 4px;">Green soft pulsing</font>** >> battery charging.

## Firmware

The Smart Citizen Kit firmware is comprised of two parts: 1) the primary processing tasks are done by the SAMD21 microcontroller firmware; 2) the tasks related to network communication are run through the ESP8266. The SAMD21 is built on top of the Arduino Zero with a custom variant for the Data Board main MCU. The ESP8266 is also built using the Arduino ESP Core. Both firmwares are built and managed with Platform IO, an open-source IDE for embedded development. Platform IO features built-in dependency management and allows you to compile and upload both processors with a single command. Using the SWD ARM connector you can change the MCU bootloader and debug the firmware using Open Source tools.

!!! info

	Learn more about the software running inside the Data Board on the [Firmware section](/Components/Firmware). 

!!! tips "Software guides"
	Check the firmware guides and learn how to update and even modify the software:

	* [Debug the Firmware](/Components/Firmware/Guides/Debug the firmware)
	* [Edit the Firmware](/Components/Firmware/Guides/Edit the Firmware)
	* [Update the Firmware](/Components/Firmware/Guides/Update the firmware)

## Buses

### Sensor Boards connector

The Kit features a modular architecture where sensors can be updated independently by replacing any individual Sensor Board. The Sensor Boards features GPIO, ADC, I2C, UART and I2S connections at 3.3V. Currently, we only offer the Urban Sensor Board, but more boards are on the way, and you can even design and build a custom one.

!!! info "Example of a Sensor Board"
    ![](https://i.imgur.com/IqLEbIr.png)

| SAMD21 Pins | Arduino Zero Pin | SCK Pins | SCK Conector | SCK Conector | SCK Pins | Arduino Zero Pin | SAMD21 Pins |
|-------------|------------------|----------|--------------|--------------|----------|------------------|-------------|
| GND         | GND              | GND      | 16           | 15           | GND      | GND              | GND         |
| GND         | GND              | GND      | 14           | 13           | GND      | GND              | GND         |
| PA11        | 0                | I2S_FS   | 12           | 11           | TX       | A5               | PB2         |
| PA7         | 9                | I2S_SD   | 10           | 9            | RX       | 25               | PB3         |
| PA10        | 1                | I2S_SCK  | 8            | 7            | VBAT     | VBAT             | VBAT        |
| PA22        | 20               | SDA      | 6            | 5            | PWM_CO   | 13               | PA9         |
| PA23        | 21               | SCL      | 4            | 3            | PWM_NOX  | 14               | PA8         |
| VCC         | VCC              | VCC      | 2            | 1            | VCC      | VCC              | VCC         |

### Auxiliary connector

The Data Board features and independent configurable auxiliary bus at 3.3V with a SEEED Studio Grove connector. The Bus has native support for I2C, but it can also be setup on firmware as a GPIO or UART port. It can supply power up to 750mA, and it can be enabled or disabled by software.

!!! info "Example of devices connected via the **AUX connector.**"
	![](https://i.imgur.com/RRu8MiV.jpg)

## Power management

The power infrastructure of the Smart Smart Citizen Kit gives us the possibility of running directly from a USB power source with or without lithium battery, it is composed by:
* [BQ24259 USB Charger](http://www.ti.com/lit/ds/symlink/bq24259.pdf) that manages external power regulation, battery fast charging (up to 2Ah) and USB OTG that allow us powering other devices from the SCK.
* [BQ27426](http://www.ti.com/lit/ds/symlink/bq27426.pdf) Battery Fuel Gauge for precise battery level measuring.

Normaly the SCK uses a 2000 mAh [Lithium polymer battery](https://en.wikipedia.org/wiki/Lithium_polymer_battery) but it is possible to take advantage of larger batteries. The charging current is regulated with a manual imposed limit that can be configured, and also auto adjusts to the connected USB charger capacity. The normal time for completely charging the stock battery is between 2 and 3 hours. It is also possible to use solar panel (5v) to charge the SCK.

The power consumption of the kit depends on which sensors are enabled and how often they are read/published. Between readings the kit goes to _sleep mode_ turning off almost all the subsystems and reducing the power consumption.

In previous versions of the kit (V2.0 and before), the most power-hungry sensors were the SGX MICS gas sensors (NO~2~ and CO) which need an always-on heater with a permanent consumption of around 50 mAh (35 hours per charge). In V2.0 and V2.1, the PM sensor which needs a fan with a consumption of 35 mAh (50 hours per charge). To improve the power consumption, the PM Sensor works on _one-shot mode_ which turns the sensor off for 3/4ths of the time, and only taking a reading after the sensor has stabilised.

The kit will operate normally: read sensors, post, and then go to sleep. Until the battery charge is below 3%. When that threshold is passed it will enter an emergency sleep mode and interrupt all the normal functions until the charge goes over 5%.

### User feedback

The charging and battery state information is shown through the led, with small flashes of different color depending on the state:

* USB cable connected
    * Orange flash - the battery **is charging**
    * Green flash - **charge complete** _you can disconnect the kit_
* No USB cable is connected
    * No flash - **charge is over 15%**
    * Orange flash and normal led Behavior (blue or pink breath) - **charge is under 15%** _connect the charger!_
    * 3 ultra fast red flashes and NO other color - **charge is under 3%** (emergency sleep) _connect the charger!! NO readings are taken!!_

### Power consumption

The base power consumption of the device is 16mA (no sensors or wifi connection). While posting data online, the consumption can go up to 75mA accounting for the ESP8266, with an additional 90mA if all the sensors are to be working at the same time (Urban Sensor Board + PM sensor).

## Source files

<a class="github-button" data-size="large" href="https://github.com/fablabbcn/smartcitizen-kit-20/archive/master.zip" data-icon="octicon-cloud-download" aria-label="Download from GitHub">Download</a>

<a class="github-button" data-size="large" href="https://github.com/fablabbcn/smartcitizen-kit-20" aria-label="Check the source code">Check the source code</a>


