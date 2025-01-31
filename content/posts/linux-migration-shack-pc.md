+++
slug = 'linux-migration-shack-pc'
title = 'Successful Migration of my Shack PC to Debian Linux'
date = 2023-10-12T15:28:00+00:00
draft = false
tags = ["Ham Radio", "Linux", "QO100"]
+++


The latest news about Microsoft's plans to use artificial intelligence on Windows machines to scan images and text for malicious content and upload them to Microsoft in case of suspicion, as well as to upload unsolicited user data to OneDrive for security purposes, has finally motivated me to migrate my Shack PC to Debian Linux.

I've been using Debian Linux for over 20 years, but even if I hadn't, installing Debian 12 with Cinnamon as the desktop environment would have been easy.

## Initial Situation

My station is based on both a Hermes Lite 2 transceiver for shortwave operation and a [Pluto+ to work the QO-100 satellite](/qo100-pluto-sdr/). For both I successfully used the very sophisticated Windows application [SDR Console](https://www.sdr-radio.com/console). It leaves nothing to be desired except that it is very resource hungry.

Already in the past I went through all alternatives to the [SDR Console](https://www.sdr-radio.com/console), but none of them seemed to be an adequate replacement.

I have now decided to use the following applications:

## Quisk

James Ahlstrom's [Quisk](https://james.ahlstrom.name/quisk/) is actually tailor-made for use with the Hermes Lite 2 and fully supports it. However, I never gave it a real chance in the past, partly because of its rustic appearance. However, after using it for a while, you'll discover features you missed in the beginning, which are hidden in unexpected places.

![](/img/linux-migration-shack-pc-1.png)

The application runs stable, requires few resources and is quickly set up with the help of the good video tutorial [Hermes-Lite 2.0 Quisk Setup](https://www.youtube.com/watch?v=1pPbQplSBoo). I had no problems with the audio, my headset was recognized directly and the first SSB QSOs resulted in good audio reports. One of these first QSOs was even a Hermes Lite to Hermes Lite QSO to VK.

## QO-100 Linux Transceiver

The [QO-100 Linux Transceiver](https://amsat-dl.org/en/qo-100-linux-transceiver/) is a fully software-based transceiver for small computer boards with an Adalm Pluto running Linux. It would even run on a Raspberry Pi 3 but without beacon synchronisation. In order to use this brilliant feature, which was the main selling point of the [SDR Console](https://www.sdr-radio.com/console) for me, you need at least a Raspberry Pi 4. I am using a quite powerful mini desktop PC and it runs very smoothly on it. Aside from the urgently needed beacon synchronisation feature, it supports RX audio muting during transmit, split operation, AGC and voice compression. 

![](/img/linux-migration-shack-pc-2.png)

First SSB QSOs went smoothly and resulted in good audio reports except that my signal was lacking in the higher frequencies. An equalizer is not implemented but I can live without this.

A detailed documentation on all included features can be found [here](http://wiki.amsat-dl.org/doku.php?id=en:plutotrx:overview).

## Logging

Logging was absolute no issue since I am a very happy [Cloudlog](https://www.magicbug.co.uk/cloudlog/) user, which is a web application that only requires a browser to be accessed.

## Verdict and Outlook

I am very happy with the performance of both applications when operating SSB. In the next step i will try to interface the two applications with WSJT-X in order to be able to do operations in digimodes as well. 
