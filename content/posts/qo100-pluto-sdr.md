+++
slug = 'qo100-pluto-sdr'
title = 'Building a QO-100 SDR Ground Station'
date = 2023-04-12T00:00:00+00:00
draft = false
tags = ["DIY", "Ham Radio", "QO100"]
+++
Frustrated by local QRM on HF, I finally decided to build a ground station for the geostationairy satellite Es'hail 2 aka Qatar-OSCAR 100 - or short - QO-100. Since I don't own a full duplex capable SSB VHF/UHF transceiver and did not want to purchase such an expensive radio, the remaining option was to go down the SDR route.

After searching the web, I've found a setup based on the Pluto+ SDR transceiver which I've then built. It consists of the following components:

* Pluto+ SDR
* Analogue Devices CN0417 pre-amp
* SG Labs 2.4GHz v3 power amplifier
* 12V to 5V DC-DC converter (for Pluto+ and CN0417)
* 12V to 26V DC-DC converter (variable, for power amplifier)
* metal enclosure for industrial use, 30x25x15 cm
* 80mm fan
* lots of SMA adapters

The components have been mounted on the metal plate that came with the enclosure. To fit the enclosure, I've chosen the following layout:

![](/img/qo100-pluto-sdr-1.jpg)

For mounting the Pluto+ I've remixed a 3D printable holder that can be found [on Printables](https://www.printables.com/de/model/488365-pluto-plus-sdr-mount).

The 20W PA can handle up to 70C but I'd rather like to have it cooled down. First test also have shown that the Pluto, which is sitting next to the PA, gets pretty warm from the radiated heat of the power amp. The solution was to cut a 80mm hole into the enclosure's door right in front of the PA and mount a fan on the inside. The enclosure is mounted indoors so I did not have to take care of waterproofing. The air is blown from the front directly onto the PA and then flows out on the top side of the enclosure wich is open.

![](/img/qo100-pluto-sdr-2.jpg)

I have chosen the following tried and tested configuration for the antenna:

* 80 cm offset dish
* Bullseye LNB
* Ice Cone Helix antenna with 3.5 turns
* 5m Ecoflex 10 coax


![](/img/qo100-pluto-sdr-3.jpg)