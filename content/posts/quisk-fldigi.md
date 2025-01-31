+++
slug = 'quisk-fldigi'
title = 'Quisk and Fldigi on Debian Linux'
date = 2024-07-02T09:00:00+00:00
draft = false
tags = ["Ham Radio", "Quisk", "Fldigi", "Linux"]
+++

After far too long, I've finally managed to get [Fldigi](http://www.w1hkj.com/) and [Quisk](https://james.ahlstrom.name/quisk) to work together to finally do digi modes like Olivia. The problems I had before were the following:

* When I used hamlib to set the frequency from Fldigi and switch Quisk from RX to TX via PTT, it seemed like Quisk and Fldigi were fighting each other. The frequency then jumped back and forth for so long that Fldigi became so unusable that I had to terminate the process .
* I did not see the sound devices within Fldigi

The [guide by James Ahlstrom](https://james.ahlstrom.name/quisk/docs.html#Digital) helped a lot, which unfortunately I initially hadn't understood in as much detail as I should have.

I have implemented the following instructions with the following software versions:

* Debian 12 stable with Cinnamon as DE
* Fldigi 4.1.23
* Quisk 4.2.35

## Quisk: Sound Settings

The following screenshot shows how I've set up the audio settings in Quisk:

![](/img/quisk-fldigi-01.png)

The two important settings are:

* Digital Tx0 Input: "__pulse: Use name QuiskDigitalInput__"
* Digital Rx0 Output: "__pulse: Use name QuiskDigitalOutput.monitor__"

## Fldigi: Rig Control

I've switched from hamlib to flrig / xmlrpc with the following settings:
* Enable flrig xcvr with Fldigi as client
* Shutdown flrig with fldigi
* Addr: 127.0.0.1
* Port: 12345
* Flrig PTT keys modem

![](/img/quisk-fldigi-02.png)

## FlDigi: Soundcard - Devices

Check the option "__PulseAudio__":

![](/img/quisk-fldigi-03.png)

## Gluing the Sound Stuff together

Now comes the interesting part!

First, start both, Quisk and Fldigi. It is very important that the applications are running. 

Now start the __Pulse Audio Volume Control__ application with the command _pavucontrol_. You might need to install it first from the  package repository.

Click on the tab "__Recording__". There should be a line visible with the Fldigi icon and the text "__capture (some number)__". Select "__Monitor of QuiskDigitalOutput__" in the drop down box next to it:

![](/img/quisk-fldigi-04.png)

Click on the tab "__Playback__". You will notice that there is no such line with the Fldigi icon visible. This is because __it is only there when you transmit from inside Fldigi__. Pick a clear frequency and e.g. send a longer CQ message in Olivia. As soon as Fldigi switches to TX, a new line with the Fldigi icon and the text "__playback (some number)__" will be shown in pavucontrol. Select "__QuiskDigitalInput__" in the drop down box next to it:

![](/img/quisk-fldigi-05.png)

## That's it!

Now you are good to go: It should now be possible to change the frequency in both Quisk and Fldigi for both applications at the same time, PTT from Fldigi should work without any problems and you should be able to see the signals received by Quisk in the Fldigi waterfall and also be able to make transmissions.
