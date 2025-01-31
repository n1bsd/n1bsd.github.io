+++
slug = 's9-generator'
title = 'Building the Box73 S9 Level Generator Kit'
date = 2024-12-22T07:00:00+00:00
draft = false
tags = ["Ham Radio"]
+++

Recently, I purchased the **S9 Level Generator Kit** from [Box73.de](https://www.box73.de). The device came as a soldering kit, but I opted not to include the optional enclosure in my order.  The S9 Level Generator is a tool designed for calibrating and testing shortwave radio equipment. The device is described on the website as follows:

"*This device is a kit for an S9 standard level generator. It is particularly useful for evaluating interference on shortwave bands using a simple software-defined receiver, provided that the receiver has been calibrated with an S9 standard level generator, such as this one. Furthermore, the generator is also suitable for checking the S-meter display of other shortwave receivers.*"

In short, the generator produces a standardized signal (S9 level) that allows you to calibrate S-meters and ensure your shortwave equipment is providing accurate readings.

## Assembling the Kit

The kit arrived with all the components and a well-documented instruction manualwhich made the assembly process smooth:

![](/img/s9-generator-01.jpg)

After soldering, I double-checked all connections and verified the board against the schematic to ensure there were no mistakes.

![](/img/s9-generator-02.jpg)


## Designing a Custom Enclosure

Since I did not purchase the optional enclosure, I decided to design my own using Tinkercad. I aimed for a compact and simple design as I am not very good with CAD software. For example, I designed the lid to be glued to the enclosure instead implementing a fancy snap-on design.

![](/img/s9-generator-03.jpg)

After designing the case, I 3D-printed it in PLA and glued it together:

![](/img/s9-generator-04.jpg)

![](/img/s9-generator-05.jpg)

The label came with the kit and gives it a nice touch. It's a plastic enclosure and therefore not shielded but it at least protects the PCB and doesn't look too shabby.

You can download the design [here](https://www.printables.com/model/1117333-box73-s9-signal-generator-case).

## Testing the S9 Generator

Once assembled, I tested the generator with four transceivers to check its calibration accuracy. The generated signals are spread over the whole HF sprectrum (1 to 30 MHz) in 100 KHz steps.

All measurements have been taken by someone who is not very experienced in this (me), RF gain is set to 100% (if applicable) and pre-amp is off:

1. **Xiegu G90**  
   Result: S9

2. **Yaesu FT-710**  
   Result: a little bit below S8

3. **Hermes Lite 2 + Quisk**  
   Result: S7

4. **QRP Labs QCX**  
   Result: over S9 (12 bars, was able to decrease the S-meter's sensitivity without change to the S-meter bar)

![](/img/s9-generator-06.jpg)
*(screenshot of the Yaesu FT-710 while testing)*

![](/img/s9-generator-07.jpg)
*(photo of the Xiegu G90 while testing)*

As far as I trust the generator, there are major differences between the tested devices. I was particularly surprised by the test result of the FT-710. 

Building the S9 Standard Level Generator was fun and it now helps me to give reports with more confidence - especially with homebew and/or SDR transceivers


