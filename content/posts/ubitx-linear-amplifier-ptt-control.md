+++
slug = 'ubitx-linear-amplifier-ptt-control'
title = 'uBitx Linear Amplifier PTT Control'
date = 2024-06-20T11:27:00+00:00
draft = false
tags = ["Ham Radio", "uBitx", "QRP"]
+++

It was time for me to get out my [HF Signals uBitx v6](https://www.hfsignals.com/index.php/ubitx/) again and use it for stationary shortwave radio operation. In the past I have already extended the uBitx with an [analogue S-meter](/ubitx_smeter/) and an [AGC board](/ubitx_agc/) as well as [adapting the firmware to my needs](/files/ubitxv6.tar.gz). Now I wanted to connect the little transceiver to my power amplifier, but unfortunately it has no external PTT output. Since the power amplifier has an active low input, i.e. it simply wants to be short-circuited at the PTT input, I immediately thought of installing a relay and controlling it somehow. An internet search led me to a [forum post in which the K1 relay was mentioned](https://groups.io/g/BITX20/topic/ubitx_v6_amp_control/85043740). This TX relay is ideal for tapping a 12V signal in order to subsequently switch another relay.

The following image shows the location of the K1 relay:

![K1 relay](/img/ubitx-linear-amplifier-ptt-control-01.jpg)

I decided to solder a cable to the relay from below. To do this, the device must first be completely dismantled and the main circuit board removed. 

The following picture shows the soldered two-wire cable:

![image](/img/ubitx-linear-amplifier-ptt-control-02.jpg)

Now I could reassemble everything, but it made sense to drill a hole in the rear panel for the RCA socket before doing so:

![image](/img/ubitx-linear-amplifier-ptt-control-03.jpg)

![image](/img/ubitx-linear-amplifier-ptt-control-05.jpg)

Now that everything was back in place and assembled, I connected a simple 5V (yes, I know...) relay module for a test. When the PTT button is pressed, the relay closes as desired and triggers the amplifier to its TX state.

![image](/img/ubitx-linear-amplifier-ptt-control-04.jpg)

The temporary relay will be swapped with a 12V module that has a opto isolator integrated as soon as it is delivered.
