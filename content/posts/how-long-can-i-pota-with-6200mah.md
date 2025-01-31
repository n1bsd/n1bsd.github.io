+++
slug = 'how-long-can-i-pota-with-6200mah'
title = 'How long can I POTA with 6200mAh?'
date = 2023-01-02T00:00:00+00:00
draft = false
tags = ["Ham Radio", "Battery", "POTA"]
+++
I've recently built two battery packs, one with eight 26650 cells (14.V, 6200mAh, originally 3300mAh/cell) and one with 24 26650 cells (14.4V, 12500mAh, originally 2400mAh/cell). You can read more about them here: [Battery Pack Building](/battery-pack-building/).

Please note that the battery cells are at least 5 years old.

## Setup

To find out how long the smaller pack with 6200mAh would last during portable operations such as POTA activations, I've set up the following experiment:

 * Transceiver: Xiegu X5105 (set to 20m, SSB and 3W output power, using internal battery)
 * Antenna: Dummy load
 * Amplifier: [MX-P50M](/mx-p50m-mod/)
 * [Microphone with built-in voice keyer](/interfacing-the-bx-184-microphone-with-the-xiegu-x5105/)


![](/img/how-long-can-i-pota-with-6200mah-1.jpg)


The voice keyer was configured to play a recorded "CQ CQ etc." message that is excatly 10s long followed by a pause of 10s. The idea was to simulate SSB communication with alternating 10 seconds of transmitting and receiving. It would run forever without any timeout in this configuration.

## Results

I've taken measurements every 30 minutes that can be taken from the following table:

| Minutes      | Voltage RX | Voltage TX  | consumed charge | PA output | TRX battery |
| ----------- | ----------- | ----------- | ----------- | ----------- |----------- |
| 30  | 13.3V | 13.0V | 360mAh  | 42W | 97% |
| 60  | 13.2V | 12.9V | 724mAh  | 42W | 94% |
| 90  | 13.1V | 12.8V | 1084mAh  | 42W | 91% |
| 120  | 13.0V | 12.8V | 1446mAh  | 42W | 90% |
| 150  | 12.9V | 12.8V | 1774mAh  | 42W | 89% |
| 180  | 12.9V | 12.8V | 2142mAh  | 42W | 88% |
| 210  | 12.9V | 12.8V | 2580mAh  | 42W | 88% |
| 240  | 12.9V | 12.8V | 2800mAh  | 42W | 88% |
| 270  | 12.8V | 12.7V | 3223mAh  | 40W | 86% |
| 300  | 12.7V | 12.5V | 3564mAh  | 39W | 86% |
| 330  | 12.6V | 12.5V | 3815mAh  | 39W | 0% |
| 360  | 11.8V | 11.5V | 4161mAh  | 35W | 0% |

The internal battery of the Xiegu dropped below 10V after 320 minutes which led to an automatic shutdown. If you take the reported percentage into account, there seems to be something wrong with either the battery or the measurement.

I've stopped the experiment after 360 minutes when both the output power of the amplifier and the battery voltage has dropped significantly.

## Remarks

Each (factory fresh) cell had 3300mAh which should result in 6600mAh for the pack. The cells were used and I measured a capacity of 6200mAh with the help of an electric load. During the above experiment the power meter measured a consumption of 4161mAh but when I recharged the battery afterwards, the charger claimed to have charged 6202mAh.

## Verdict

The battery seems to be a good fit for portable operations for to 5.5 hours. I was honestly surprised how stable the battery voltage is over the time of the discharge. The internal battery of the Xiegu also has a sufficient reserve for around 5 hours. I am not sure how much I can trust the measurements of the el cheapo battery power meter, the electric load or of the charger. The only truth, however, is the result in hours of experimentation.