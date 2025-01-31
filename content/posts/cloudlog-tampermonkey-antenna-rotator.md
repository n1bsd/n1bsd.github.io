+++
slug = 'cloudlog-tampermonkey-antenna-rotator'
title = 'Automating Antenna Rotator Control with Cloudlog and Tampermonkey'
date = 2024-02-13T10:36:00+00:00
draft = false
tags = []
+++

I've recently built a [Remote Control for the Hy-Gain AR-500X Antenna Rotator](/remote-control-hygain-antenna-rotator/) which works nicely but requires me to look up the desired heading for a specific station in Cloudlog, go to the remote control web application, determine the best memory channel and then click it to move the rotator.

Determined to streamline this process, I embarked on a journey to automate antenna control with the help of Cloudlog and a little scripting magic.

**Disclaimer:** I've never heard of [Tampermonkey](https://www.tampermonkey.net/) before and I am also not the biggest fan of JS. The resulting work might offend people with more knowledge.

## The Solution

Inspired by the potential of automation, I decided to leverage the power of [Tampermonkey](https://www.tampermonkey.net/), a popular userscript manager, to enhance the functionality of Cloudlog. My goal was simple: to add a clickable link below the gridsqaure field in the QSO entry window that would automate the process of adjusting my antenna rotator based on the direction of my QSO partner.

## How It Works

The magic lies in the custom JavaScript injected into the Cloudlog webpage by the [Tampermonkey](https://www.tampermonkey.net/) script. With this script in place, whenever I log a new QSO in Cloudlog, the heading info below the gridsquare field is converted into a clickable link. Upon clicking this link, the script reads the heading info provided by Cloudlog (via QRZ.com) and sends a GET request to the [remote control system for my antenna rotator](/remote-control-hygain-antenna-rotator/).

The remote control system receives the GET request, extracts the desired heading, and selects the nearest memory channel to adjust the antenna accordingly. 

## The Script

You can find the script [here](/files/hygain-ir-controller.tar.gz).
