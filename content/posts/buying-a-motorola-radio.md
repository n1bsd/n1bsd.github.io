+++
slug = 'buying-a-motorola-radio'
title = 'What you should know before buying a Motorola Radio'
date = 2024-07-01T11:55:00+00:00
draft = false
tags = ["Ham Radio", "Motorola"]
+++

This post is not a detailed guide to buying a Motorola radio but rather a list of things that I think you as a ham should know about before buying a used device. I am also not a Motorola expert. My experience is based solely on the purchase of two Motorola SL1600 (SL300 in the US). The following are my observations and comments:

If you buy a used Motorola, you will need a commercial __Customer Programming Software (CPS)__ to be able to program it. Especially with a device like the SL1600, which has neither a proper display nor a keypad, nothing can be done without a CPS (I am sure that there are Motorolas that you can program on the radio itself but I have no experience with them). This program is either expensive to purchase or can be downloaded illegally from shady web sites. The Motorola CPS is __Windows software__, so there is no way around a Windows system for programming. It furthermore is annoyingly bad and has limitations depending on the version as described further down.

I would strongly recommend __buying__ such radios __only from radio amateurs__. Here you can be relatively sure that it is not a device that comes directly from the commercial sector. Sometimes devices are sold that have been used in supermarkets, for example, and then find their way onto classified ad portals via more or less legal channels. The people who then sell them usually know nothing about the programming or the general condition of the device and cannot be sure whether the device is password-protected. If the latter is the case, the buyer is faced with a problem (more details on this later). The seller is furthermore most likely not aware of which __licenses__ have been bought for this particular device. The features activated via licenses are not only important for operation, but also a decisive factor for the price. In my opinion, the "5-Tone Signalling" and "RX Audio Leveling" are useful licenses. The former is needed, for example, to open a relay with a 1750Hz tone, the latter is particularly useful in DMR operation, as not every ham is able to set a decent audio level.

I had initially thought that I could simply buy a second SL1600 and then conveniently write the same code plug on both devices. Unfortunately, this is __not possible if the devices have different licenses installed__. This is also the transition to the next, much more serious problem: __If a device is password-protected and you don't have the password, you can't read out the code plug__. If you now think that you can simply create a code plug in the CPS and write it to the device, you are mistaken. You must either have a suitable code plug or read it out of the device. The password-protected device thus initially becomes a paperweight. The options known to me are as follows:

* __Buying a code plug:__ You buy a suitable code plug from a web shop that exploits an over-commercialized system even more commercially

* __Reading out the password__: If you connect the device to the computer via the programming cable, the CPS communicates with the radio via a virtual network interface. If the device has a very old firmware, it is easy to read out the password using Wireshark, as the radio sends the password in clear text to the CPS for verification. Newer firmware versions have fixed this "issue".

* __Patching the CPS__: If you have the CPS version 16 and a radio with a slightly older firmware than the latest one (which is therefore still supported by CPS 16), you can use a hex editor to modify a DLL in the software so that the CPS still asks you for the password, but accepts any password you enter. You can then read the code plug, remove the password and write it back. The older CPS 16 is the better, faster and less space consuming software in comparison to CPS 20 but might not be compatible anymore with radios that have the most recent firmware.

For legal reasons, I cannot provide links to all of these programs, information, etc.

If I notice anything else, I will update this post.




