+++
slug = 'automatic-antenna-switch-system'
title = 'Cheap and simple fully automatic antenna switching'
date = 2023-01-13T00:00:00+00:00
draft = false
tags = ["3D Printing", "Antenna", "Ham Radio"]
+++
## Preconditions

The HF part of my amateur radio station mostly consists of the following:

 * Transceivers: ICOM IC-7300 and a Zachtek WSPR Desktop Transmitter
 * Antennas: Cobweb antenna for 6 to 20m, Hustler 4-BTV for 40m
 * Antenna switches: One coax relay in the shack (shack antenna switch) for selecting which transceiver is connected to the antenna, one remote antenna switch close to the antenna (remote antenna switch) for selecting which antenna will be connected to the coax relay in the shack

## Goals of this project

A solution should be found that achieves the following:

 * If the IC-7300 is powered on
   * the WSPR transmitter should be switched off
   * the shack antenna switch should be switched to the IC-7300
 * If the IC-7300 is powered off
   * the WSPR transmitter should be switched on
   * the shack antenna switch should be switched to the WSPR transmitter
   * the remote antenna switch should be switched to the Hustler 4-BTV
 * If the band is switched in the IC-7300
   * the Cobweb antenna should be selected for all bands between 20m and 6m
   * the Hustler 4-BTV should be selected for all bands between 160m and 40m

In other words: I'd like to fully automatically transmit WSPR packages via the Hustler 4-BTV when I am not using the station. If I am using it, the appropriate antenna for the current band should be selected.

## Possible solutions

Multiple possible solutions come into mind:

 * A Raspberry Pi (or similar SBC) could be connected to the USB port of the IC-7300 and the frequency could be read out via CAT. A connected relay board could be used for controlling the antenna switches
   * Downsides: Raspberry Pis are expensive, have boot times, OS needs maintanance, timeouts might be waited for in order to detect if the IC-7300 has been powered off, error prone, lots of overhead
 * A solution with an Arduino could be implemented where the Arduino reads the voltage from the BAND pin of the ACC port (same could be done with the Raspberry Pi).
   * Downsides: Needs custom programming, still costs some money

## An alternative approach

Since I only have two antennas and one of them is for the upper parts of the bands while the other one is for the lower parts, the following very simple and affordable solution came into mind:

### Automatic switching of the shack antenna switch

The IC-7300's ACC port provides a 13,8V output pin which is specified to be able to deliver up to 1A. The shack antenna switch is directly connected to this pin and will therefore toggle its state depending on the power state of the IC-7300. The coax cables are connected in a way that the WSPR transmitter is selected when no voltage is applied to the coax switch.

Furthermore the WSPR transmitter needs to be powered off as soon as the IC-7300 has been switched on. For this a simple 12V relay board (~3,50 EUR) will also be connected to the 13,8V output pin of the IC-7300's ACC port. The WSPR transmitter is powered via a Micro-USB cable from which the red wire will be cut and wired to the relay board so that it is connected to the pins of the relay that are closed when no voltage is applied to the relay. This way the WSPR transmitter will be powered with 5V via an external power adapter when the IC-7300 is powered off and be shut down when the IC-7300 is powered on.

### Automatic switching of the remote antenna switch

A cheap battery monitor (~11 EUR) - which are normally used to protect batteries from deep discharge by switching them off when a certain voltage as been reached - will be connected to the BAND pin of the ACC port. One can freely define the cut off voltage in the variant I've purchased. Depending on the selected frequency band, a voltage of between 0 and 8 volts is applied to the BAND pin. I've configured mine to trigger at a voltage of 4,5V since the value for 20m is 4V and the one for 40m is 5V.

## Diagram

The following diagram shows how all the above mentioned components are wired together:

![](/img/automatic-antenna-switch-system-1.jpg)


## Pictures

Both PCBs have been mounted on a custom designed and 3D printed adapter board which again has been mounted to the bottom of the enclosure.


![](/img/automatic-antenna-switch-system-2.jpg)


The finished device:


![](/img/automatic-antenna-switch-system-3.jpg)


![](/img/automatic-antenna-switch-system-4.jpg)


![](/img/automatic-antenna-switch-system-5.jpg)


## Parts used

 * Enclosure: Donau Elektronik - KGB15 Euro Box klein, Blau, 95x135x45, ~5 EUR
 * Relay Board: Hailege 2pcs 12V 1 Channel Relay Module Relay Switch With Optocoupler Isolation, ~3,50 EUR each
 * Battery Monitor: DONGKER Low Voltage Shutdown Protects DC 6-40V Digital Battery Monitor Undervoltage Protection Module, ~11 EUR
 * Coax Relay: CX600M, pretty expensive, I already owned them
 * Cables
 * Connectors: ACC plug came with the IC-7300, rest was "in stock"
 * 12v plug-in power supply
 * 5V USB power supply plus USB cable
 * [3D printed PCB adapter board](https://www.thingiverse.com/thing:5791063)
