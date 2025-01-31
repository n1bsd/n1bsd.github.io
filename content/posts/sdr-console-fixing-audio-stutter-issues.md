+++
slug = 'sdr-console-fixing-audio-stutter-issues'
title = 'SDR Console: Fixing Audio Stutter Issues'
date = 2023-04-14T00:00:00+00:00
draft = false
tags = ["Ham Radio", "SDR", "Windows"]
+++
# The Setup

* Lenovo Thinkpad T420
* Intel Core i5-2520M CPU
* 16 GB RAM
* Intel 82579 Ethernet Interface
* Windows 10 Pro
* [SDR Console v3.3 Beta Build 2870](https://www.sdr-radio.com/Console)

# The Problem

I had lots of audio stuttering/drop-outs while using [SDR Console] on my Thinkpad T420 (see above), even if no other programs were running.

# The Fix

In order to find out if there are device driverrs that are blocking other processes due to high DPC latency spikes, I've downloaded and executed the tool [LatencyMon](https://resplendence.com/latencymon).

The following screenshot shows the main screen of LatencyMon running:

![](/img/sdr-console-fixing-audio-stutter-issues-1.jpg)

The drivers with the highest execution times and/or numbers of DPC executions can be seen unter "Drivers":

![](/img/sdr-console-fixing-audio-stutter-issues-2.jpg)


In my case it turned out that the _ndis.sys_ driver was the main issue with extreme executions times (not visible in the screenshot, took it after the fix). After reading [this very informative blog post](https://geekhack.org/index.php?topic=82387) I've replaced the generic network driver for Intel cards with a much older version from Lenovo (from 2011). The execution times are now still not great but much lower and I haven't experienced any audio stutter problems in SDR Console anymore.

You can download the Lenovo driver from here: https://download.lenovo.com/ibmdl/pub/pc/pccbbs/mobiles/83rw15ww_64.exe