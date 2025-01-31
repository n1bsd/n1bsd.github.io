+++
slug = '70cm-eggbeater-antenna'
title = 'Building a LEO SAT ground station: 70cm Eggbeater Antenna'
date = 2023-05-02T08:56:23.045768+00:00
draft = false
tags = ["Antenna", "Ham Radio", "Satellite", "LEO"]
+++
After I am now finally QRV on the geostationary satellite QO-100, I would like to be able to additionally work LEO satellites, which have amateur radio transponders on board. These are earth-orbiting satellites, which can only work for a short period per pass between the AOS (Acquisition Of Signal - when the SAT rises above the horizon) and the LOS (Loss Of Signal - when it passes below the horizon).

The most efficient variant would be to acquire e.g. two Yagi antennas with an azimuth/elevation rotator. However, this system is expensive, complex and requires maintenance. A cheaper and less maintenance variant are omnidirectional antennas, which are circularly polarized, for example the Eggbeater antenna. I decided to use exactly this type of antenna for this project. These antennas are available for purchase, but unfortunately they are also quite expensive.

Most earth orbiting amateur radio satellites have a transponder on the 2m and the 70cm band, of which one is used for the uplink and the other for the downlink. Which band is used for which direction of transmission varies from satellite to satellite.

To make the Eggbeater antenna project even easier and more cost efficient, I decided to build the Eggbeaters myself, but to design the 70cm Eggbeater dedicated for the downlink and the 2m variant for the uplink. This post is about building the 70cm antenna.

These two excellent documentations by ON6WG and F5VIF served as a basis for the construction:

* [Antenne-Eggbeater-Engl-Part1-Full.pdf](/files/Antenne-Eggbeater-Engl-Part1-Full.pdf)
* [Eggbeater-Ant-Revisited-English.pdf](/files/Eggbeater-Ant-Revisited-English.pdf)

Further information on this design can be found on [M0AWS' blog](https://m0aws.co.uk/?p=2106).

Fortunately, there is a 3D printable design for the 70cm Eggbeater antenna after ON6WG and F5VIF, which can be found on [Thingiverse](https://www.thingiverse.com/thing:4737130).

I printed all needed parts in PETG over several days. Then I redesigned the lower part so that it now has a flange to which the same part can be screwed upside down. The idea behind this is to allow a better way to mount it to a pole. I will publish the changes soon as a remix on Thingiverse.

The following pictures show details of the construction of the printed antenna, in the first picture the production of the phasing line:


![](/img/70cm-eggbeater-antenna-1.jpg)


Here is a picture showing the assembled phasing line together with the feed line:


![](/img/70cm-eggbeater-antenna-2.jpg)


And here the finished assembled top part of the antenna:


![](/img/70cm-eggbeater-antenna-3.jpg)


The following picture shows the loosely assembled antenna without wiring:


![](/img/70cm-eggbeater-antenna-4.jpg)

After the antenna was fully assembled, I installed it on a mast at about 6m height and connected it with 7m Aircell 7 coaxial cable via a 70cm mast preamplifier to a RTL-SDR stick. This in turn is plugged into a Raspberry Pi 3b running OpenWebRX.

I am very satisfied with the performance of the 70cm Eggbeater so far. Currently I only have a Diamond X200 vertical antenna available for transmitting, but with this combination (70cm Eggbeater for RX, Vertical for TX) I could already make 3 QSOs over the ISS during a single pass.

Here you can find a short video of my first test after the assembly: [https://www.youtube.com/shorts/19CcW8kmTn0](https://www.youtube.com/shorts/19CcW8kmTn0). Please note that I've changed the antenna design from rectangular wires to the classic round version during the tuning process.

Currently I am redesigning the 70cm Eggbeater to make it a printable 2m version. As soon as this is finished, another post will follow with the details of the construction and the results of my tests.