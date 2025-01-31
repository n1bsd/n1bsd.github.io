+++
slug = 'interfacing-the-bx-184-microphone-with-the-xiegu-x5105'
title = 'Interfacing the BX-184 Microphone with the Xiegu X5105'
date = 2022-12-04T00:00:00+00:00
draft = false
tags = ["Ham Radio", "Xiegu", "Modification"]
+++
I wanted to use my BX-184 microphone which has a built-in voice keyer with my new Xiegu X5105. It was used with the FT-817 until now and I enjoyed the parrot very much during my POTA activations. I've read that the Xiegu would use the same wiring as the IC-7000 so I followed the instructions of the manual (https://www.box73.de/file_dl/bausaetze/BX-184_en.pdf, page 3, bottom right). The mic itself worked but PTT was not. Analysing the original microphone, I realized that I need to connect MSVSW (Pin 3) with GND (Pin 7). Now PTT, and with this, the parrot is working, too.

![](/img/interfacing-the-bx-184-microphone-with-the-xiegu-x5105-1.jpg)


This is how my BX-184 is now wired:


![](/img/interfacing-the-bx-184-microphone-with-the-xiegu-x5105-2.jpg)

The green wire is the additional connection that has to be done in order to get PTT working.