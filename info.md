# Home Assistant integration for XY Screens projector screens and lifts

![Python][python-shield]
[![GitHub Release][releases-shield]][releases]
[![Licence][license-shield]][license]
[![Maintainer][maintainer-shield]][maintainer]
[![Home Assistant][homeassistant-shield]][homeassistant]
[![HACS][hacs-shield]][hacs]  
[![Github Sponsors][github-shield]][github]
[![PayPal][paypal-shield]][paypal]
[![BuyMeCoffee][buymecoffee-shield]][buymecoffee]
[![Patreon][patreon-shield]][patreon]

## Introduction

Home Assistant integration to control XY Screens and See Max projector screens and lifts over the
serial and RS-485 interface, or via TCP network connections using RS-485-to-Ethernet converters.

This Home Assistant integration was first implemented for XY Screens. After I was informed that the
See Max devices use a very similar protocol support for these devices has been added.

[XY Screens](https://www.xyscreen.com/) and See Max are OEM manufacturers of projector screens and
lifts, their devices are sold around the world under various brand names.

## Features

- Installation/Configuration through Config Flow UI
- **Serial and TCP network connections** - Connect via USB RS-485 adapters or RS-485-to-Ethernet converters
- Set the up and down duration of your projector screen/lift
- Position control, move the screen/lift to any position along the way
- Use multiple devices on the same RS-485 interface
- Invert the default Cover Entity behaviour

### About position control

The XY Screens and See Max projector screens and lifts do not provide any positional feedback. The
state of the screen is thus always an assumed one. The screen position is calculated based on the
time the cover has moved and the configured up and down durations. This results in a potential
error margin. Every time the screen reaches it maximum up or down position the position and thus
any potential error is reset accordingly. If the screen is controlled outside of Home Assistant,
for instance with the remote control, the screen position and state will no longer represent the
actual state.

## Hardware

### Serial Connection (USB RS-485)

I use a cheap USB RS-485 controller to talk to the projector screen where position 5 of the RJ25
connector is connected to D+ and position 6 to the D-.

![image](https://raw.githubusercontent.com/rrooggiieerr/homeassistant-xyscreens/main/usb-rs485.png)

### Network Connection (RS-485-to-Ethernet)

Alternatively, you can use an RS-485-to-Ethernet converter to connect your screen to your network.
This eliminates the need for a direct serial connection and allows remote control over TCP/IP.

Compatible and tested converters:
* [ebyte NA111-E](https://www.cdebyte.com/products/NA111/4#Downloads)

Configure your converter with these settings:
- **Baud Rate**: 2400
- **Data Bits**: 8
- **Parity**: None
- **Stop Bits**: 1
- **TCP Server Mode** with your chosen port (commonly 9997)

See the documentation of your specific device on how to wire yours correctly.

## Supported protocol

If your devices follows the following protocol it's supported by this Home Assistant integration:

2400 baud 8N1  
Up command  : `0xFF 0xXX 0xXX 0xXX 0xDD`  
Down command: `0xFF 0xXX 0xXX 0xXX 0xEE`  
Stop command: `0xFF 0xXX 0xXX 0xXX 0xCC`

Where `0xXX 0xXX 0xXX` is the three byte address of the device.

For XY Screens devices the default address is `0xAA 0xEE 0xEE`, while for See Max devices the default
address is `0xEE 0xEE 0xEE`.

## Supported projector screens and lifts

The following projector screens is known to work:

- iVisions Electro M Series

The following projector screens and lifts are not tested but use the same protocol according to the
documentation:

- iVisions Electro L/XL/Pro/HD Series
- iVisions PL Series projector lift
- Elite Screens
- KIMEX
- DELUXX
- Telon

See Max:
- ScreenPro
- Monoprice
- Grandview
- Dragonfly
- WS Screens
- Cirrus Screens
- Lumien
- Celexon

Please let me know if your projector screen or lift works with this Home Assistant integration so I
can improve the overview of supported projector screens and lifts.

## Caution

This integration follows the Cover Entity where open means raising the screen and close lowering
the screen, like how roller blinds, garage doors and curtains work. For a projector screen this is
counter intuitive. You can chose to invert this behaviour when adding your screen or lift to Home
Assistant. The dahsboard will then show the screen controls inverted, arrow up will lower the
screen while arrow down will raise the screen. However the voice commands Open and Close will then
work as expected.

Thanks to the power of Home Assistant translations the entity state in a non-inverted screen will
show correctly, however the voice commands and actions are inverted.

## Adding a new XY Screens projector screen or projector lift

<img src="https://raw.githubusercontent.com/rrooggiieerr/homeassistant-xyscreens/main/screenshot_add_device.png" width="305"/>

- After restarting go to **Settings** then **Devices & Services**
- Select **+ Add integration** and type in **XY Screens**
- Choose your connection type:
  - **Serial Port (RS485 USB adapter)** - for direct USB connections
  - **Network (RS485-to-Ethernet converter)** - for TCP network connections
- Configure your connection:
  - **Serial connection**: Select the serial port or enter the path manually
  - **Network connection**: Enter the IP address and port (e.g., `192.168.1.100:9997`)
- Select the address of your device or enter the address manually
- Select the type of device, projector screen or projector lift
- Set the up and down times of your device
- Optionally invert the cover behavior if desired
- Select **Submit**

A new XY Screens integration and device will now be added to your Integrations view.

## Contributing

If you would like to use this Home Assistant integration in youw own language you can provide me
with a translation file as found in the `custom_components/xyscreens/translations` directory.
Create a pull request (preferred) or issue with the file attached.

More on translating custom integrations can be found
[here](https://developers.home-assistant.io/docs/internationalization/custom_integration/).

## Support my work

Do you enjoy using this Home Assistant integration? Then consider supporting my work using one of
the following platforms, your donation is greatly appreciated and keeps me motivated:

[![Github Sponsors][github-shield]][github]
[![PayPal][paypal-shield]][paypal]
[![BuyMeCoffee][buymecoffee-shield]][buymecoffee]
[![Patreon][patreon-shield]][patreon]

## Hire me

If you would like to have a Home Assistant integration developed for your product or are in need
for a freelance Python developer for your project please contact me, you can find my email address
on [my GitHub profile](https://github.com/rrooggiieerr).

[python-shield]: https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54
[releases]: https://github.com/rrooggiieerr/homeassistant-xyscreens/releases
[releases-shield]: https://img.shields.io/github/v/release/rrooggiieerr/homeassistant-xyscreens?style=for-the-badge
[license]: ./LICENSE
[license-shield]: https://img.shields.io/github/license/rrooggiieerr/homeassistant-xyscreens?style=for-the-badge
[maintainer]: https://github.com/rrooggiieerr
[maintainer-shield]: https://img.shields.io/badge/MAINTAINER-%40rrooggiieerr-41BDF5?style=for-the-badge
[homeassistant]: https://www.home-assistant.io/
[homeassistant-shield]: https://img.shields.io/badge/home%20assistant-%2341BDF5.svg?style=for-the-badge&logo=home-assistant&logoColor=white
[hacs]: https://hacs.xyz/
[hacs-shield]: https://img.shields.io/badge/HACS-Default-41BDF5.svg?style=for-the-badge
[paypal]: https://paypal.me/seekingtheedge
[paypal-shield]: https://img.shields.io/badge/PayPal-00457C?style=for-the-badge&logo=paypal&logoColor=white
[buymecoffee]: https://www.buymeacoffee.com/rrooggiieerr
[buymecoffee-shield]: https://img.shields.io/badge/Buy%20Me%20a%20Coffee-ffdd00?style=for-the-badge&logo=buy-me-a-coffee&logoColor=black
[github]: https://github.com/sponsors/rrooggiieerr
[github-shield]: https://img.shields.io/badge/sponsor-30363D?style=for-the-badge&logo=GitHub-Sponsors&logoColor=#EA4AAA
[patreon]: https://www.patreon.com/seekingtheedge/creators
[patreon-shield]: https://img.shields.io/badge/Patreon-F96854?style=for-the-badge&logo=patreon&logoColor=white
