+++
slug = 'hermes-lite-2-to-hardrock-50-interface-v2'
title = 'Hermes Lite 2 to Hardrock-50 Interface V2.0'
date = 2023-10-12T07:30:52.055777+00:00
draft = false
tags = ["Amplifier", "Arduino", "Ham Radio"]
+++
The following older project already solved my problem with my constantly freezing Hardrock-50 amplifier by interfacing it with the Hermes Lite 2 with the help of an Arduino Nano as an I2C to serial translator: [Hermes Lite 2 Interface Box (HR50 and Antenna Switch)](/hl2-hr50-interface).

Things have changed in between as I do not want to automatically switch the antenna anymore. The antenna is now switched via Home Assistant and a ZigBee smart plug which gives me much more flexibility - but that's another story.

It was now time to get rid of the additional box and cables so it was decided to integrate the interface into the HL2 itself:

![](/img/hermes-lite-2-to-hardrock-50-interface-v2-1.jpg)

The following is a list of parts used for this project:

* Arduino Mini Pro 3.3V 8MHz
* 2x20 female pin header
* 9 pin female serial port
* small stripboard

The idea is still the same as in the older project: The I2C bus of the HL2 is read by the Arduino by grabbing the signal from the N2ADR 2x20 pin connector, the band information is then decoded and the corresponding serial commands send to the Hardrock-50 which is directly connected via a serial cable.

I picked the Arduino Mini Pro 3.3V 8MHz because I only had a 3.3V pin available on this connector and I did not want to introduce a voltage converter into the circuit.

The following is the wiring diagram for the interface:

![](/img/hermes-lite-2-to-hardrock-50-interface-v2-2.png)

The code is still the same and can be found [here](/files/hl2-interface.tar.gz).
