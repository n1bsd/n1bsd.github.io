+++
slug = 'hardrock50-remote-display'
title = 'Hardrock-50 Remote Display'
date = 2021-11-12T00:00:00+00:00
draft = false
tags = ["ESP32", "Ham Radio", "Hardware", "Python"]
+++
UPDATE: I moved the client and the server into [one project](/files/hr50-remote-display.tar.gz). It furthermore now supports user actions via a button.


Original post:

I am using my [Hardrock-50](/building-the-hardrock50/) as part of a remote station so I am of course not able to monitor its display which shows the selected band, keying method, temperature, SWR, power etc. Since I need/want this information and also prefer a physical display, I made two little projects that can be combined together:

### HR50-API

This is a small python script that connects from e.g. a Raspberry Pi to the HR50 via USB. When it is started, it spawns a web server which then provides two API endpoints in your network. Those endpoints can be queried via HTTP requests to:

* remotely gather information from the HR50
* remotely execute commands

It is basically a serial to HTTP converter. You can find the code and more information here: [hr50-api.tar.gz](/files/hr50-api.tar.gz)

### HR50-Remote-Display

This project consists of a Arduino sketch that can be flashed to a ESP32 module with OLED display and wifi connectivity. It uses the above mentioned API to gather information from the HR50 over wifi and displays them on the OLED.


![](/img/hardrock50-remote-display-1.jpg)


This is how the device looks like in action:


![](/img/hardrock50-remote-display-2.jpg)


I plan to add buttons so that e.g. the keying mode can be changed via this device.  The projects are only two days old so I will certainly put more love into it. There are several things missing like error handling, comments etc.

You can find the code and more information here: [hr50-remote-display.tar.gz](/files/hr50-remote-display.tar.gz)
