+++
slug = 'm17-md380-openrtx-pistar-mmdvm'
title = 'M17 with the TYT MD380 + OpenRTX + Pi-Star + MMDVM'
date = 2021-12-13T00:00:00+00:00
draft = false
tags = ["Ham Radio", "Modification", "M17"]
+++
In this post I will explain in detail how to become QRV on M17 with the following setup:

 * [TYT MD380](https://www.miklor.com/MD380/)
 * Raspberry Pi / [Pi-Star](https://www.pistar.uk/) / MMDVM Hotspot

The process will be the following:

 * Modification of the MD380: In order to transmit M17 audio, the radio needs to be modified so the MCU has direct access to the mic without the audio being routed over the HR_C5000.
 * Installation of OpenRTX on the MD380
 * Modification of the Pi-Star installation
 * Upgrading the MMDVM firmware to at least v1.6.0

## Disclaimer and Thanks

First of all: None of this is my work. It is just a documentation of how I implemented the work of others. Furthermore, I would not have come far without the help of multiple devs and users of the M17 and OpenRTX project. Some but not all of them are DL5SMB, IU2KWO, IU2KIN, IU5BON, tarxvf. Thank you!

Another thing: If you follow these instructions you might:

 * destroy your MD380
 * destroy our MMDVM hat
 * kill a lot of time
 * have a lot of fun

Furthermore, **the MD380 will only be able to transmit audio** and will not be able to transceive/decode yet. This will be added in future OpenRTX releases.

## MD380 Mod

This is how the device looks opened but still unmodified:


![](/img/m17-md380-openrtx-pistar-mmdvm-1.jpg)


The modification of the TYT MD380 is described [here in great detail](https://openrtx.org/#/md380_mods) and would normally need no further explaination. If you compare the images on the linked website with how my modification looks like, you will notice some differences:


![](/img/m17-md380-openrtx-pistar-mmdvm-2.jpg)


**Difference 1:** I don't have any SMD components, which is why I performed the "Mic -> MCU" mod (upper part in the picture) with a regular resistor. Make sure that the resistance is positioned in such a way that it does not press against the display during / after assembly.

![](/img/m17-md380-openrtx-pistar-mmdvm-3.jpg)


**Difference 2:** I messed up with the "RF -> MCU" mod (lower part in the picture) and damaged a solder pad:


![](/img/m17-md380-openrtx-pistar-mmdvm-4.jpg)


The alternative is to take some wire and connect pin 1 of U101 with pin 18 of the STM32. I needed a microscope for this to solder...


![](/img/m17-md380-openrtx-pistar-mmdvm-5.jpg)


## Flashing OpenRTX to the MD380

Please follow the instructions [of the OpenRTX user's manual](https://openrtx.org/#/user_guide).
I flashed the [latest nightly build](https://openrtx.schinken-radio.de/nightly/openrtx_md3x0_wrap).

## Patching Pi-Star

Connect to your Pi-Star via SSH and switch the filesystem to rw mode:

```
# rpi-rw
```

Now update the system to the latest version:

```
# sudo pistar-update
# sudo pistar-upgrade
```

Afterwards, use [IU5BON's patch script (Github)](https://github.com/nolith/Pi-Star_Binaries_sbin/wiki) to path your Pi-Star installation with everything needed for M17:

```
# curl https://raw.githubusercontent.com/nolith/Pi-Star_Binaries_sbin/M17/pistar-install-m17 | sudo bash -
```

The following command starts the M17 Gateway service:

```
# sudo systemctl start m17gateway.service
```

Now enable M17 in your dashboard:


![](/img/m17-md380-openrtx-pistar-mmdvm-6.jpg)


Afterwards, configure the M17 gateway service to your likings. You can find the settings under "Expert/M17 GW". The URL is: http://<HOTSPOT_IP>/admin/expert/fulledit_m17gateway.php

![](/img/m17-md380-openrtx-pistar-mmdvm-7.jpg)


## Updating MMDVM

The following steps might vary depending on what MMDVM modem you are using. I am using the default Chinese eBay variant that can be plugged on eg. on a Raspberry Pi Zero and is often sold in combination with a metal enclosure and a OLED screen, the MMDVM_HS_HAT.

Connect to your Pi-Star via SSH and switch the filesystem to rw mode:

```
# rpi-rw
```

Install needed tools and libs:

```
# sudo apt-get update
# sudo apt-get install gcc-arm-none-eabi gdb-arm-none-eabi libstdc++-arm-none-eabi-newlib libnewlib-arm-none-eabi
```

Clone the source into the local file system:

```
# git clone https://github.com/juribeparada/MMDVM_HS
```

There is a library inside this repository linked as submodule. We need to take care that it will be available during compile time:

```
# cd MMDVM_HS/
# git submodule init
# git submodule update
```

Edit the file _Config.h_ and adapt all parameters to your needs. I edited/checked the following params:

```
#define MMDVM_HS_HAT_REV12 //defines which board you use
#define ENABLE_ADF7021
#define ADF7021_14_7456 // use this if you have the 14MHz tcxo
#define STM32_USART1_HOST
#define ENABLE_SCAN_MODE
```

Now we need to stop some services since they will interfere with the flashing process:

```
# sudo pistar-watchdog.service stop
# sudo systemctl stop mmdvmhost.timer
# sudo systemctl stop mmdvmhost.service
```

We can now compile and flash the new firmware:

```
# make clean
# make
# sudo make mmdvm_hs_hat
```

## Usage

### Using the MD380

Before using the MD380, you need to set the following things:

 * Enter your callsign:
   * Press the green button
   * Select "Settings"
   * Enter call
 * Change mode to M17
   * Press and hold the button below the PTT button
   * Press 5 until it shows "M17"
 * Enter the frequency of your hotspot in the main VFO screen

### Receiving Audio

Since this solution has so far only made it possible to send and not to receive M17 audio with the MD380, you also need an additional software like [Droidstar](https://github.com/nostar/DroidStar). This way you can send with the MD380 and receive via Droidstar. Decoding M17 audio will be added in future releases of OpenRTX.

### Switching the Reflector

You can switch to another reflector with the following command (Thanks DL5SMB!):

```
# echo "ReflectorM17-M17 C" | nc -u 127.0.0.1 6075
```

## Links

The following is a collection of links that are helpful and/or necessary to do the above described:

 * [OpenRTX User Manual](https://openrtx.org/#/user_guide?id=how-to-flash-openrtx-to-your-radio)
 * [MD380 mod](https://openrtx.org/#/md380_mods)
 * [OpenRTX nightly builds](https://openrtx.schinken-radio.de/nightly/)
 * [M17 Protocol Specification](https://m17-protocol-specification.readthedocs.io/en/latest/)
 * [https://github.com/mathisschmieder/MMDVM_HS_Hat/blob/master/README.md](https://github.com/mathisschmieder/MMDVM_HS_Hat/blob/master/README.md)
 * [MMDVM HotSpot: firmware for ZUMspot or MMDVM_HS based boards (D-Star, DMR, YSF, P25, NXDN and POCSAG)](https://github.com/juribeparada/MMDVM_HS)
 * [https://github.com/juribeparada/MMDVM_HS/blob/master/BUILD.md](https://github.com/juribeparada/MMDVM_HS/blob/master/BUILD.md)
 * [Pi-Star M17 Patch Script](https://github.com/nolith/Pi-Star_Binaries_sbin/wiki)
 * [https://github.com/g4klx/M17Gateway](https://github.com/g4klx/M17Gateway)
 * [https://m17project.org/integrate/](https://m17project.org/integrate/)
 * [https://www.dl1drk.de/?p=1010](https://www.dl1drk.de/?p=1010)