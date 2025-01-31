+++
slug = 'mat-1500pro'
title = 'mAT-1500Pro: An emotional rollercoaster - or how I destroyed and repaired a brand-new ATU'
date = 2024-12-03T10:00:00+00:00
draft = false
tags = ["Ham Radio", "Repair"]
+++

**Update 2024/12/23:** I've managed to kill the ATU a second time and learned some more. More on that at the end of this article.

Since I have a small HF amplifier, I tune my [Delta Loop](/m0plk-delta-loop/) with a manual tuner from MFJ. Lazy as I am, I looked around for an automatic tuner that can handle at least 600W. In the end I chose the mAT-1500Pro. The two reviews on eHam that existed at the time were both 5 star reviews and promised the best. However, I also read a few comments under a YouTube video that said that the tuner often only finds very poor matches. In Germany, however, you have a 14-day right of return on items ordered over the internet, which is why I saw no risk here and bought it.

After I received the tuner, I connected it to my Yaesu FT-710 via the separately available cable and tested it. The first tests were very sobering. The internal tuner of the Yaesu is able to find a good match on every band, the mAT mostly only found matches like 2.5. After some testing I decided to send the tuner back, but I still wanted to test it with the amplifier. I made a mistake and accidentally tuned it briefly at full power. This ATU is only capable of being tuned with a maximum of 15W. The tuner only passes the PTT signal to the amplifier when it is not tuned, but unfortunately my amplifier has an HF sensing module...

Suddenly the electronic smell of failure and despair was in the air. The tuner still worked afterwards, but the matches were even worse. So I opened the device, removed the sensor board and realised that at least one resistor was damaged. 

![](/img/mat-1500pro-1.jpg)

The sensor board is vertically placed on the back of the tuner:

![](/img/mat-1500pro-6.jpg)

Close-up of the sensor board:

![](/img/mat-1500pro-2.jpg)

Close-up of R20:

![](/img/mat-1500pro-3.jpg)

So I had two options: Repair the sensor board or buy a new one. MAT-TUNER has contact information on their website which I used to ask for a wiring diagram. I also tried to enquire whether they would sell me a sensor board separately. Unfortunately, I never received an answer. The local dealer was also unable to help me with this and even told me that MAT-TUNER doesn't give circuit diagrams away.

## First Repair Attempt

Now that I was on my own, the only option left was to attempt a repair. The depressing thing was that even if I managed to do it, I would end up with a tuner that I would no longer want because of the experiences I made before the incident.

First step was to desolder the resistors R18 to R21:

![](/img/mat-1500pro-4.jpg)

The affected 49,9 Ohm SMD resistors have a very uncommon size (1812) which I couldn't find anywhere in the EU so I had to order them from Mouser in the USA. The package with 10 resistors (which then ended up costing 27€) arrived only 2 days later. I then soldered them back onto the board, which was not easy because of the two neighbouring heat sinks:

![](/img/mat-1500pro-5.jpg)

After reassembling everything, I've tested again and the result was sadly exactly the same as with the old 4 resistors R18 to R21. I've then talked to [Mike, M0AWS](https://m0aws.co.uk/) and he suggested to see what these two heat-sinked components next to R18 and R19 are. So I've desoldered the sensor board again from the main board and desoldered the two components with the heat sinks. To my surprise these two components are also resistors (R1 and R2). 

![](/img/mat-1500pro-7.jpg)

These TO220 packaged resistors have markings that suggest resistances of 75 Ohm (R1) and 100 Ohm (R2). I've measured 120 Ohm at the 75 Ohm resistor and 19.4 Ohm at the 100 Ohm resistor. As this does not correspond to the labelling of the resistors, the hope arose that this could be the cause of the problem.

Even if replacing the four SMD resistors did not bring any direct success, it probably made sense to replace at least the smelly and externally damaged one.

## Second Repair Attempt

Next step was to order new TO220 resistors with 75 Ohm (Caddock MP930 75 1%) and 100 Ohm (Caddock MP930 100 1%) and a rating of 30W. 

As soon as they arrived, I've mounted them into the two heatsinks with some fresh thermal paste and soldered them back into the board.

![](/img/mat-1500pro-8.jpg)

It actually worked now - and even better than it did before my mistake with which I damaged the sensor board!

![](/img/mat-1500pro-9.jpg)

## Review

Now that the tuner was working again (and actually better), i was able to integrate it back into my shack. The ATU must first be tuned on all bands at regular intervals. I decided to do this in 5 kHz steps. Unfortunately, it didn't always find a good match, but after retuning several times it was able to find it. After this tedious work was completed, I was rewarded with a tuner that was well tuned on all frequencies. The best thing about it is that you don't have to press PTT and send a signal for the tuner to recall the stored match. If you turn the VFO knob, you can hear (very) loud relay clicks as the ATU sets the appropriate match for each frequency during reception thanks to its CAT connection.

![](/img/mat-1500pro-10.jpg)

Unfortunately, I do not have any long-term experience with the mAT-1500Pro, and despite the happy ending, I cannot recommend it. If you have a problem with the device, you are on your own. There doesn't seem to be a large user base and the manufacturer doesn't care about existing customers. It furthermore only survives tuning with up to 15W which isn't really much for a 1500W ATU.

## Update 2024/12/23: Second Incident

I killed both 30W resistors again by accidently tuning with the amp in active state. The 75 Ohm resistor measured 371 Kilo Ohm afterwards, the 100 Ohm resistor 806 Ohm. The ATU worked fine again after replacing them. **Both resistors measure roughly 50 Ohm each in-circuit when they are fine.**
