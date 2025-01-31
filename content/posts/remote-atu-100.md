+++
slug = 'remote-atu-100'
title = "Remote ATU based on the ATU-100"
date = 2024-08-29T09:00:00+00:00
draft = false
tags = ["Ham Radio", "QDX", "CubeSat", "QRP", "3D Printing"]
+++

![](/img/remote-atu-100-01.jpg)

I've already built some [ATU-100](https://github.com/Dfinitski/N7DDC-ATU-100-mini-and-extended-boards) automatic antenna tuners [in the past](/atu-100/): One was a build for QRO and one for QRP with only 5 windings on the binocular transformer and adaptions to the original firmware. While the QRO version worked fine, I had issues with the QRP variant. After I've discovered the very nice [alternative firmware of DG4SN](https://github.com/DG4SN/ATU-100-EXT-YAF) I've decided to build an ATU-100 based phantom powered remote antenna tuner that can be mounted on the mast directly beneath my [M0PLK Delta Loop](/m0plk-delta-loop/). The nice thing with DG4SN's firmware is that it allows QRP operation with the original 10 windings on each side of the transformator.

I found a nice waterproof enclusure with the perfect measurements but without any possibilities to mount it to a mast. So the first thing to do was to add four M4 brass threads that were melted into the ABS enclosure on the back:

![](/img/remote-atu-100-02.jpg)

Two 3D printed t-shaped parts were then screwed to the enclosure. With them, the enclosure can be mounted to the mast with zip ties or big hose clamps:

![](/img/remote-atu-100-03.jpg)

To be able to mount the ATU-100, the Bias-T and the display inside the enclosure, I've designed a simple inlay and printed it in PETG:
 
![](/img/remote-atu-100-05.png)

This is how the tuner looks like with all components mounted and wired together:

![](/img/remote-atu-100-04.jpg)

I sadly had to use hotglue because the screw head of one of the screws that hold the display broke off.
