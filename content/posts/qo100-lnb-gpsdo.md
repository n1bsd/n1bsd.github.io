+++
slug = 'qo100-lnb-gpsdo'
title = 'QO-100: Adding a second GPSDO as an LNB clock reference'
date = 2023-10-30T00:00:00+00:00
draft = false
tags = ["GPSDO", "Ham Radio", "QO100"]
+++

After recently [migrating my Shack PC from Linux to Debian Linux](/linux-migration-shack-pc/), I can no longer use the great software "SDR Console". This has the feature of calibrating and stabilising the receive frequency using the QO-100 middle beacon. That's why I wanted to make my QO-100 setup software-independent. 

To achieve this, I exchanged my Bullseye LNB for an modified Diavolo Twin LNB, where the second output now serves as an input for an external clock reference. First I connected an indoor TCXO to it. This was quite stable, but always required manual corrections. The choice finally fell on a second Leo Bodnar Mini GPSDO. The [existing GPSDO serves as a clock reference for the Pluto+ SDR transceiver](/qo100-gpsdo/).

The following measurement of the GPSDO with the oscilloscope shows that the output signal is not a nice sine wave, which is needed by the LNB:

![](/img/qo100-lnb-gpsdo-1.jpg)

The following configuration items of the Leo Bodnar Mini have been adapted for this particular setup:

* Drive setting: 8 mA (+6.4dBm)
* Frequency: 24999999 Hz

I've chosen this frequency over the default 25 MHz because [other hams seem to have a problem with the exact frequency of 25000000 Hz](https://forum.amsat-dl.org/index.php?thread/205-strange-lnb-behaviour-with-gpsdo/).

I've found [a nice filter designed and sold by DF1QE](https://web.archive.org/web/20240612152906/https://elektronik-muenster.de/thread/8268-digitale-referenz-an-lnb-anpassen/) that solves this problem:

![](/img/qo100-lnb-gpsdo-3.jpg)

This is the filter installed in a metal enclosure and connected to the GPSDO during the tests on the workbench:

![](/img/qo100-lnb-gpsdo-4.jpg)

With the filter in place, the signal looks like it should do:

![](/img/qo100-lnb-gpsdo-2.jpg)

The finished package:

![](/img/qo100-lnb-gpsdo-5.jpg)

The first QSOs showed that the RX and TX frequencies are remarkably close together and super stable. After setting an initial small offset for the LNB frequency, no further manual interaction is necessary anymore.
