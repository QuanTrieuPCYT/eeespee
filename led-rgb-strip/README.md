# LED RGB Strip
A Tuya-powered, RGBCCT, analog and 2-meter LED RGB Strip with a smart controller. Sold by [ACOME](https://www.acomeofficial.com) as "ACOME AL03 Smart LED Strip". Being listed [here](https://akia.vn/thiet-bi-smarthome/den-thong-minh/den-led-day-16-trieu-mau/den-led-day-hieu-ung-thong-minh-rgb-acome-al03).

## Hardware
The controller has the Beken BK7231T chip, which I strongly believe to be a [Tuya WB3S module](https://docs.libretiny.eu/boards/wb3s). Probing shows the GPIO pins for proper functioning:

| GPIO | Function           |
|------|--------------------|
| 1    | Button             |
| 9    | Red Channel        |
| 24   | Green Channel      |
| 26   | Blue Channel       |
| 8    | Cold White Channel |
| 6    | Warm White Channel |

## Flashing
Device came with an old with unpatched vulnerabilities that can be used for flashing over OTA by using [tuya-cloudcutter](https://github.com/tuya-cloudcutter/tuya-cloudcutter).\
Unlike [tuya-convert](https://github.com/ct-open-source/tuya-convert), [tuya-cloudcutter](https://github.com/tuya-cloudcutter/tuya-cloudcutter) **does not** backup your original firmware. Please resort to hardware flashing and do a flash backup if you ever want to use Tuya firmware again!\
With that in mind, here is the wiring required for serial flashing (I haven't dissected the device yet because I managed to flash it via OTA, if you want to go for this route it is best to grab a soldering iron as it might not have nicely exposed pads or testpoints):

| Programmer | CB2S |
|------------|------|
| TX         | RX   |
| RX         | TX   |
| GND        | GND  |
| 3V3        | VCC  |

The Beken chip when waking up will see if there's activity present on the TX and RX pin, if yes it puts itself into download mode. If the chip isn't getting recognized by your programmer, try momentarily pulling `CEN` to `GND` or manually reseating the connection.

## Features
The LED controller is located behind my bed, so physically interacting with it is not an usual thing for me. The LED controller functions exactly like the original firmware, but now with the lightweight and benefits of ESPHome not sending my traffics to some random cloud!\
The light can also be quickly toggled **when the button is first held in**, not after releasing the button like the original firmware so it will be faster too.

## Notes
Wi-Fi Power Saving Mode is disabled to fix flickering + improve connection stability.