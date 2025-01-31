+++
slug = 'remote-control-hygain-antenna-rotator'
title = 'Remote Control for the Hy-Gain AR-500X Antenna Rotator'
date = 2024-02-03T00:00:00+00:00
draft = false
tags = ["Antenna", "Ham Radio", "Remote", "Rotator"]
+++

The project described here attempts to solve the following problem: an antenna is to be rotated remotely using a Hy-Gain AR-500X antenna rotor. The rotor comes with a control unit for this purpose. However, the aim is to develop a remote control for the control unit and to make the control unit network-compatible so that it can be controlled by a computer via the network.

![](/img/remote-control-hygain-antenna-rotator-3.jpg)

## Preconditions

* The original antenna rotor control unit should remain unmodified
* A bidirectional antenna is installed, so the rotator only needs to be able to rotate 180 degrees, not the full 360 degrees.

## The Idea

The control unit has an infrared receiver that was originally intended for the infrared remote control supplied. This was replaced in this project by an ESP8285 microcontroller board, the _Hailege ESP8285 ESP-01M Digital Infrared Transceiver_, which also has an infrared transmitter and receiver module integrated on the circuit board.

As I only need a rotation of 180 degrees and the controller has 12 memory locations from A to L, I have distributed the headings to the available memory locations as follows:

| Memory | Heading 1 | Heading 2 |
| ---- | ---- | ---- |
| A | 0 | 180 |
| B | 15 | 195 |
| C | 30 | 210 |
| D | 45 | 225 |
| E | 60 | 240 |
| F | 75 | 255 |
| G | 90 | 270 |
| H | 105 | 285 |
| I | 120 | 300 |
| J | 135 | 315 |
| U | 150 | 330 |
| L | 165 | 345 |

## The Code

You can find the Arduino sketch here: [hygain-ir-controller.tar.gz](/files/hygain-ir-controller.tar.gz)

The resulting web application is not really beautiful but functional:

![Screenshot of the web application](/img/remote-control-hygain-antenna-rotator-1.jpg)

Every button has a letter from A to L assigned. A click on a button of the web application sends the corresponding infrared code to the HyGain control unit, selects this specific memory channel and turns the antenna to the pre-programmed direction.

Follow the resulting GET request to select the memory location G:

```
http://<IP_ADDRESS>/get?memory=G
```

The webserver can also be given a numerical value for the desired heading. The closest memory location is then selected. The request looks like this for a heading of 210 degrees: 

```
http://<IP_ADDRESS>/get?heading=210
```

## Hardware Implementation

No soldering was necessary as the _Hailege ESP8285 ESP-01M Digital Infrared Transceiver_ already has everything on board that is needed. I've mounted the board with a zip tie to a board right above the sensor of the base unit and connected it to a 5V power source.

![The base control unit](/img/remote-control-hygain-antenna-rotator-2.jpg)
