+++
slug = 'qo100-gpsdo'
title = 'Adding a GPSDO to the Pluto+'
date = 2023-04-13T00:00:00+00:00
draft = false
tags = ["DIY", "QO100", "Ham Radio", "GPSDO"]
+++
While [SDR Console](https://www.sdr-radio.com/Console) is doing a great job stabilizing the frequency with the help of locking to the middle beacon of the QO-100 narrow band transponder, it still has some inaccurancy. Because of this and because I might try out other SDR software in the future, I've decided to enhance my [QO-100 ground station](/qo100-pluto-sdr/) with a GPSDO. The product I have chosen for this is the well known "Mini Precision GPS Reference Clock" from Leo Bodnar.

First thing to do is to add a SMA socket to the Pluto+. There is a small IPEX UFL socket on the board next to the built-in TCXO. I've ordered a 10cm long IPEX to SMA female cable, drilled a 6mm hole above the 5V micro USB socket and installed the cable by connecting it to the IPEX UFL socket and mounting the SMA socket into the newly drilled hole.

![](/img/qo100-gpsdo-1.jpg)


In addition to this it is also required to set a jumper to connect EXCLK to GND so the Pluto+ knows we want to use an external clock reference (Please see [https://github.com/plutoplus/plutoplus](https://github.com/plutoplus/plutoplus)).


![](/img/qo100-gpsdo-2.jpg)


Before connecting the GPSDO to the Pluto+, it should be configured first to deliver an output frequency of 40000000Hz with the help of the configuration software that can be found on the official product web page. It is also a good idea to reduce the output drive strentgh to the lowest needed settings, in my case 8mA.


![](/img/qo100-gpsdo-3.jpg)


After connecting everything and powering up the Pluto+, it is necessary to connect to the Pluto+ via SSH and execute the following commands to make it work:

```
fw_setenv refclk_source external
fw_setenv ad936x_ext_refclk_override 40000000
```

As soon as the GPSDO has gained its GPS lock, the Pluto+ is then properly frequency stabilized with the help of the external GPSDO.