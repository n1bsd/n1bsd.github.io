+++
slug = 'm0plk-delta-loop'
title = 'Construction of a M0PLK Delta Loop Antenna'
date = 2024-01-17T16:06:00+00:00
draft = false
tags = ["Antenna", "Ham Radio"]
+++

I'm actually very happy with my Cobweb antenna for the shortwave bands from 20-10m. It is omnidirectional, small and picks up significantly less noise than my vertical. Then one day I read a lot of positive things about the M0PLK multiband delta loop antenna. One of them was from an OM who also had a Cobweb and after switching to the M0PLK found clear advantages over the Cobweb, especially in terms of picking up local QRM. Initially unsure but very interested, I discussed the topic with M0AWS, who then [published a very detailed report on the M0PLK on his website](https://m0aws.co.uk/?p=2725). He modelled the antenna on a PC and analysed it extensively. Due to the positive characteristics of the Delta Loop, I then decided to build and test this antenna. 

## The Antenna

The M0PLK antenna is a non-resonant multi band delta loop antenna that requires an antenna tuner to be usable on 20 to 10m. The original design describes two 5.65m long aluminium tubes arranged at right angles, which form the majority of the radiating parts of the antenna. The two ends protruding into the air are pulled together by the 2x 2.80m long antenna wires, which merge into a 2.30m long ladder line in the middle. This then leads vertically down to a 1:4 balun.

![Diagram of the antenna with all measurements](/img/m0plk-delta-loop-1.jpg)

## Parts

The following is a list of parts used to build this antenna:

* 1 aluminum plate 300x300x4mm
* 2 mast mounting brackets
* ~12.5m DX-Wire FS2 wire
* 6 green 25 mm hydraulic tube clamps
* 12 stainless steel screws, M4 x 40 mm
* 4 stainless steel screws, M4 x 14 mm
* 1 1:4 balun kit from HF Kits
* 2 aluminum tubes, 2000 mm long, 25 mm outer diameter, 2 mm wall thickness
* 2 aluminum tubes, 2000 mm long, 20 mm outer diameter, 1,5 mm wall thickness
* 2 aluminum tubes, 2000 mm long, 16 mm outer diameter, 1 mm wall thickness
* 6 stainless steel screw clamps
* 2 stainless steel cable clamps
* 6 M4 cable shoes
* 1 3D printed middle insulator ([.stl file](/files/M0PLK_Middle_Insulator.stl))
* 2 3D printed 16mm tube end caps for antenna wire fastening ([.stl file](/files/M0PLK_14mm_End_Cap.stl))
* 10 3D printed 65mm ladder line spacers ([.stl file](/files/M0PLK_65mm_Ladder_Line_Spacer.stl))

## Base Plate Construction

I started by mimicking the shape of the original M0PLK and cutting it out of a 300x300x4mm aluminium sheet with a jigsaw.

![The base plate after cutting and drilling](/img/m0plk-delta-loop-2.jpg)

The following diagram shows the measures I used. The dimensions may vary depending on the clamps used or the size of the balun housing. Overall, most of the dimensions are not critical, just make sure that there is a 90 degree angle between the two tubes and that the clamps are distributed in such a way that sufficient stability is guaranteed. I myself dimensioned the base plate based on estimates and simply orientated myself on the components to be placed on it.

![The base plate after cutting and drilling](/img/m0plk-delta-loop-13.jpg)

I then placed 3 of the hydraulic pipe clamps on each side at right angles to each other, marked the holes and drilled them. I drilled two 10mm holes for the mast clamps at the top and bottom. The empty housing of the 1:4 balun was then used as a drilling template to drill the four 4mm holes for attaching the balun.

![The base plate on a workbench, the balun box still opened](/img/m0plk-delta-loop-3.jpg)

## Building the Balun

Detailed instructions can be found here: [https://www.hfkits.com/manual-14-balun-600-watts/](https://www.hfkits.com/manual-14-balun-600-watts/)

## Preparing the Tubes

I cut all pipes crosswise on one side with a hand saw to a width of approx. 4cm. These slits will later be used to fix the small tubes, which are inserted 20cm on this side, using a screw clamp.

![Close-up of a screw clamp holding two tubes together](/img/m0plk-delta-loop-4.jpg)

On the other, unslotted side of the 25mm tubes, I first drilled a 3.3mm hole at the end and then cut an M4 thread. With the help of a short antenna wire, to which two cable lugs are soldered, the two tubes are finally electrically connected to each other with two screws

I proceeded in the same way with the two 16 mm pipes so that I could later connect the wire to the pipe.

I designed an end cap to attach the antenna wire to the left and right at the highest point of the antenna, i.e. at the end of the 16mm tube, which I then printed twice with PETG filament. These are inserted at the top and later fixed with a screw clamp.

## Preparing the Wires

The two wires needed for building the M0PLK are 610 cm long each. These are attached to the upper ends of the aluminium tubes on the self-printed end caps with strain relief and electrically connected to the tubes using a screw connection. The wires are then threaded through the middle insulator, which is also 3D printed, where they are strain-relieved and fixed in place.

![Close-up of one of the ladder line spacer](/img/m0plk-delta-loop-5.jpg)

![Close-up of the middle insulator](/img/m0plk-delta-loop-6.jpg)

The remaining 330 cm are fitted with ladder line spreaders at intervals of 30 cm. The cable lugs at the end of the wire are then used to create a connection to the 1:4 balun.

## Assembling the Antenna

The following picture shows the finished but yet unassembled antenna:

![The finished but yet unassembled antenna on the ground](/img/m0plk-delta-loop-7.jpg)

Here some pictures of the antenna during assembly:


![Close-up of the aluminium tube tip showing how the wire is mounted](/img/m0plk-delta-loop-8.jpg)


![Close-up picture of the base plate with all connections made](/img/m0plk-delta-loop-9.jpg)

The finished antenna in all its glory, mounted on a telescopic mast (not fully extended):

![The Delta Loop antenna mounted, view from the flat roof](/img/m0plk-delta-loop-10.jpg)


![The Delta Loop antenna mounted, view from the ground level](/img/m0plk-delta-loop-11.jpg)

## Testing

The following VSWR measurements are in line with expectations:

![A VSWR diagram made with a NanoVNA](/img/m0plk-delta-loop-12.jpg)

I've had some successful SSB test QSOs to the USA on 10, 12, 15, 17 and 20m immediately after completion, but still need to do some more experiments.

As this is a bidirectional antenna, the setup is not very useful in its current form. I will now look into a lightweight remote controlled antenna rotator to make full use of its capabilities.

## Sources

I took ideas, inspirations and measurements from the following sources:

* [Counting Radios: M0PLK Multiband Delta Loop Antenna](http://countingradios.blogspot.com/2014/02/multiband-m0plk-delta-antenna-v2012-alu.html)
* [Antena M0PLK (Polish slides as PDF)](http://cra.swidnica.pl/cra/technika/antena_M0PLK_-_prezentacja.pdf) / [Mirror](/files/antena_M0PLK_-_prezentacja.pdf)
* [SP9WOL: 14MHz Delta Loop Construction (version of M0PLK)](http://www.sp9wol.com/?p=62)
* [Assembly instructions for a commercial variant in Polish](http://www.teltad.pl/pobieranie/instrukcje/vpa-systems/delta_HF_pionowa_instrukcja_montazu.pdf) / [Mirror](/files/delta_HF_pionowa_instrukcja_montazu.pdf)
* [G4VZV](https://www.qrz.com/db/G4VZV)