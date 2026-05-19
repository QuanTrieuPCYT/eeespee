# Temperature and Humidity Sensor
A Tuya table clock, with a temperature and humidity sensor built-in. Original product link on [Aliexpress](https://vi.aliexpress.com/item/1005009493389363.html).\
Device is named "T & H Sensor" as seen on Tuya IoT Developer page. Device has MCU ID of `7akwzwfwhukkdsib`.

## Hardware
Device is powered by a [Tuya WB3S Module](https://docs.libretiny.eu/boards/wb3s), which is in turn a Beken BK7231T-based chip. A detailed teardown of the device is fortunately available [here](https://www.elektroda.com/rtvforum/topic3819498.html).\
Device has another companion MCU called TuyaMCU that runs proprietary firmware alongside the Beken chip and cannot be flashed by easy means. Fortunately the chip is only for the functionalities of the device itself (display reporting and temperature/humidity sensor reporting) so privacy is not really a concern with this device after flashing.\
The MCU talks through UART interface, with a baud rate of 9600. It exposes two datapoints for temperature and humidity sensor reporting. Configuring this is very easy because ESPHome also came with support for TuyaMCU out of the box.

| GPIO | Function      |
|------|---------------|
| 6    | Button        |
| 9    | LED Indicator |

| TuyaMCU Datapoint | Function          |
|-------------------|-------------------|
| 1                 | Temperature (*10) |
| 2                 | Humidity          |

## Flashing
Device came with an old with unpatched vulnerabilities that can be used for flashing over OTA by using [tuya-cloudcutter](https://github.com/tuya-cloudcutter/tuya-cloudcutter).\
Unlike [tuya-convert](https://github.com/ct-open-source/tuya-convert), [tuya-cloudcutter](https://github.com/tuya-cloudcutter/tuya-cloudcutter) **does not** backup your original firmware. Please resort to hardware flashing and do a flash backup if you ever want to use Tuya firmware again!\
With that in mind, here is the wiring required for serial flashing (I haven't dissected the device yet because I managed to flash it via OTA, if you want to go for this route it is best to grab a soldering iron as it might not have nicely exposed pads or testpoints):

**Note:** It is best to desolder the chip and flash it independently, as the companion TuyaMCU also communicates via the TX and RX pins.

| Programmer | WB3S |
|------------|------|
| TX         | RX   |
| RX         | TX   |
| GND        | GND  |
| 3V3        | VCC  |

The Beken chip when waking up will see if there's activity present on the TX and RX pin, if yes it puts itself into download mode. If the chip isn't getting recognized by your programmer, try momentarily pulling `CEN` to `GND` or manually reseating the connection.

## Features
Behave exactly like the stock firmware, but now with added assurance that the device will only work locally in your home automation system.\
The button at `GPIO6` is also wired to the TuyaMCU and facilitates switching from Celsius to Fahrenheit display mode so we can't do much thing with it unless we flash the TuyaMCU firmware with something custom.