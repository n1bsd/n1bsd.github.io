+++
slug = 'hl2-hr50-interface'
title = 'Hermes Lite 2 Interface Box (HR50 and Antenna Switch)'
date = 2023-05-16T16:25:31.909345+00:00
draft = false
tags = ["Antenna", "Arduino", "DIY", "Ham Radio"]
+++
**Update: I've published a post on a simpler, more integrated version [here](/hermes-lite-2-to-hardrock-50-interface-v2/).**

I have only had bad luck connecting my Hermes Lite 2 SDR transceiver directly to a Hardrock 50 power amplifier. The HR50 was constantly freezing and I did not find a solution in the HR50 user group. After I had discarded this setup for a long time, I wanted to give the HL2 a try again but only in QRP. For this I wanted to have an automatic antenna switching, which selects the the antenna in dependence of the selected frequency band. After longer research I found the very nice project of EA3IGT, the [70W Power Amplifier for the Hermes-Lite 2.x](https://github.com/ea3igt/HL2-PA70). The code he published already contains almost everything I needed for an implementation based on the Arduino Nano.

This gave me the idea to modify his solution so that I can not only automatically select the antenna, but also send the corresponding commands to change the band to the Hardrock 50. The setup looks like this:

```
Hermes Lite 2 ---I2C---> Arduino ---RS232---> Hardrock 50
                            |
                            |
                             ---> 5V relay -> remote antenna switch
```

## Preparation of the HL2

The Hermes Lite 2 is extended by a DB9 socket, with which the I2C bus is passed to the outside. All necessary pins are on the small connection board between the mainboard and the LPF board of the HL2, see [EA3IGT's documentation](https://github.com/ea3igt/HL2-PA70/blob/main/Images/HL2%20to%20PA70%20connection%20v1.0.JPG).

The following picture shows how the DB9 connector is wired inside the Hermes Lite 2:
![](/img/hl2-hr50-interface-1.jpg)

## The Software

As stated above, the code is based on the original author's work and has been adapted to my needs:

 * One digital output pin (D10) is added to trigger the relay
 * the band switching routine has been modified so that a) the correct antenna for the chosen band is selected by setting D10 to high or low and b) a command is sent to the HR50 via the Arduino's serial interface.
 * Everything else has been removed from the code

The code can be found [here](/files/hl2-interface.tar.gz).

## Putting everything together

The following diagram shows how everything needs to be wired:

![](/img/hl2-hr50-interface-2.jpg)

Description of the connectors, top to bottom:

 * 5V power supply
 * Male DB9 connector for interfacing the Hermes Lite 2. Pinout can vary and depends on how the HL2 is internally wired to its DB9 connector
 * Male DB9 connector for interfacing the Hardrock 50. Pinout is fixed and can be taken from the HR 50 instruction manual. Needs to connected with a 1:1 cable.
 * Socket with at least 3 pins for driving the external antenna switch. In my case, I've inserted 12V into the red wire so that either the upper or the lower black wire is part of the circuit.

The final result:


![](/img/hl2-hr50-interface-3.jpg)


