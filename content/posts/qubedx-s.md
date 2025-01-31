+++
slug = 'qubedx-s'
title = "QubeDX S - The QubeDX's little brother"
date = 2024-08-24T09:44:00+00:00
draft = false
tags = ["Ham Radio", "QDX", "CubeSat", "QRP", "3D Printing"]
+++
![](/img/qubedx-s-01.jpg)

After I've built the [QubeDX](/qubedx/) for the higher bands (20 to 10m) and therefore for the [M0PLK Delta Loop antenna](/m0plk-delta-loop/), I now wanted to build a second one, mostly for 40m. As my Hustler 4BTV is resonant on 40m, there was no need for a tuner. This meant that it was possible to place all necessary components into the [original 10x10x10cm design of the 3D printable CubeSat](https://www.thingiverse.com/thing:4096437).

As with the original QubeDX, the idea was to build a (decorative) QRP radio for digi mode operation that can be operated remotely via Wi-Fi by accessing the desktop via VNC. This time the costs were even more reduced, especially as I had a spare Raspberry Pi 4 lying around. This project only requires the following components:

* [QRPLabs QDX 5W digital transceiver kit](https://qrp-labs.com/qdx) (low band version)
* Raspberry Pi 4 with passive cooling
* two buck converters

Since all components fit into the original frame, only the modules are custom designed and [can be found on printables.com](https://www.printables.com/de/model/984556-qubedx-s-the-qubedxs-little-brother).

First of all, the individual components had to be assembled:

## Power Supply Module

The power supply module consists of two simple buck converters and a Powerpole connector. One buck converter is configured to put out 5V for the Raspberry Pi, the other to 7.5V for the QDX:

![](/img/qubedx-s-02.jpg)

## Transceiver Module

Now it was time to set up the QDX transceiver. Again, the unprecedentedly good assembly instructions from QRPLabs left no questions unanswered, which is why the assembly turned out to be quite simple. This time I've decided to [place the BS170s in sockets](/notes-on-the-qrp-labs-qdx/).

![](/img/qubedx-s-03.jpg)

## Raspberry Pi Module

The Raspberry Pi was mounted in sandwich construction on the corresponding module carrier:

![](/img/qubedx-s-04.jpg)

## Assembly

Now the CubeSat frame was printed and screwed together with M3x10 stainless steel screws and the corresponding nuts. The three pre-assembled module supports were then installed in the frame one above the other using M3 nylon spacers so that they were screwed to the frame with four screws each at the top and bottom. 

The cabling was then installed:

* A short USB cable to connect the QDX to the Raspberry Pi (CAT and audio)
* two two-core cables for the power supply, each running from the Powerpole connector to the buck converters
* Two two-wire cables from the buck converters to the QDX (hollow plug) and the Raspberry Pi (GPIO)

## The Result

The following pictures show the finished setup:

![](/img/qubedx-s-05.jpg)

![](/img/qubedx-s-06.jpg)

The QubeDX S in comparison to his big brother:

![](/img/qubedx-s-07.jpg)

## Updates and Modifications

I am documenting all updates and modifications [here in this separate post](/notes-on-the-qrp-labs-qdx/).
