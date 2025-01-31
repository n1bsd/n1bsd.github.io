+++
slug = 'dm4ds-midi-box'
title = 'Building the "DM4DS MIDI Box" SDR Controller'
date = 2023-05-14T00:00:00+00:00
draft = false
tags = ["3D Printing", "DIY", "Arduino", "MIDI", "SDR"]
+++
In the last time I hardly sat in front of a real radio but was mainly QRV via SDR radios (Hermes Lite 2 and Pluto Plus). These I operate with the great software SDR Console from Simon Brown. However, I missed the VFO knob and other "real knobs" on the PC and was therefore looking for a homebrew project to realize this. In the end I decided to go with the MIDI Box from [DM4DS](http://www.dm4ds.de/).


![](/img/dm4ds-midi-box-1.jpg)

The parts I was using for my build:

 * Arduino Uno R2
 * 3x 19mm Stainless steel push buttons
 * 3x rotary encoders with knobs as a set
 * 5x blue LED
 * lots of wire
 * the obligatory hot glue

## Programming the Arduino

The Arduino sketch can be downloaded from here: [https://github.com/DM4DS/MIDI_Box_v1](https://github.com/DM4DS/MIDI_Box_v1). It does not need any adaptions and can be directly uploaded to the Arduino.

The following libraries are needed to compile the sketch for the Arduino Uno:

* [Encoder](https://github.com/PaulStoffregen/Encoder)
* [Midi Controller](https://github.com/tttapa/MIDI_controller/releases/latest)

## Installing the midi firmware

Note: This is only necessary for the Uno or Mega.

Download and install the [Atmel Flip Software](https://www.microchip.com/en-us/development-tool/flip). Follow this [installation guide](https://github.com/tttapa/MIDI_controller/blob/master/README.md#installation).

Afterwards flash the firmware as described in [this document](https://github.com/tttapa/MIDI_controller/blob/master/README.md#arduino-uno-or-mega).

Please keep in mind that you cannot upload any modified/new sketeches to the Arduino any more. If you need to do so, reflash the Arduino firmware, upload the sketch and flash the midi firmware again.

## Printing the Enclosure

The STL files for printing the enclosure can be [downloaded here](https://www.printables.com/de/model/389052-midi-box-controller-fe-sdrconsole-arduino). Both enclosure parts can be printed without supports and with an ifill of 20%.

## Wiring

For wiring the Midi Box, I followed the great [schematics provided by DC5WW](https://www.dc5ww.de/midi-box/schaltplan.pdf). I did not implement his PTT LED idea but found the schematics itself very useful.

Note that (at least the Uno's) pin 13 is hard-wired to the on-board LED and can't be used as a digital input. I therefore did not wire the switch of one of the top left rotary encoders to the Arduino and used the now free pin for the VFO knob encoder.

The blue LEDs were too bright with the proposed 150 Ohm resistor so I've replaced it with a 3,3K variant.

Here are some pictures of the internal wiring:


![](/img/dm4ds-midi-box-2.jpg)


![](/img/dm4ds-midi-box-3.jpg)

Things I've hot glued:

* The big nut that is used as a weight
* All connectors going into the arduino. I've populated all pins with cut off plugs so that they hold each other better and that the hot glue stays in place while glueing
* the 5 LEDs