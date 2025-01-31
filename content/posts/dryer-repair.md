+++
slug = 'dryer-repair'
title = 'Repair of an AEG Lavatherm Dryer'
date = 2024-12-20T07:00:00+00:00
draft = false
tags = ["Repair"]
showthedate = true
+++

For several weeks, we encountered issues with our AEG Lavatherm Protex Plus dryer (Model: T5.5IH, Type: TC12H6MHP, purchased in 2014). The drying cycles were frequently interrupted by the error code EH0, which, according to AEG, indicates a problem with the power supply. 

I've read about a quite common issue that dry solder joints on the main circuit board might be the culprit, so I disassembled the dryer to inspect the board:

![](/img/dryer-repair-01.jpg)

I discovered that the pins of the black relay (marked with **1**) exhibited signs of dry solder joints:

![](/img/dryer-repair-02.jpg)

I proceeded to resolder these pins and all other through-hole components to ensure solid connections everywhere.

After reassembling the dryer, it powered on immediately. However, a new issue emerged: the dryer would operate for a short period before shutting off and becoming unresponsive. To restart it, I had to unplug the machine and wait approximately 30 minutes before it would power on again. Notably, upon reconnecting the power, the dryer would start without pressing any buttons. The error log returned an E00 code, indicating no recorded errors.

Seeking assistance, I turned to an appliance forum, where a member suggested that faulty electrolytic capacitors and/or the LNK304GN (Lowest Component Count, Energy-Efficient Off-Line Switcher IC, marked with **2**) might be causing the issue. Additionally, I ran the dryer's diagnostic program, which returned the following results:

* C2: 000
* C5: 014
* C6: 014
* C7: 111
* C8: E32
* C9: E32
* C10: E32

The persistent E32 error codes in C8, C9, and C10 raised concerns about potential sensor issues, possibly caused during my earlier cleaning of the dryer's interior.

Upon a thorough review and with the help of pictures takes before the disassembly, I discovered that during reassembly, I had plugged two of the many plugs into the wrong edge connectors on the motherboard (**3** is the correct configuration, **4** the wrong one) . Although the connectors fit interchangeably, this incorrect configuration led to the dryer's erratic behavior. After correcting the connections, the dryer resumed normal operation. However, the diagnostic program continued to display E32 errors in C8, C9, and C10, even after performing a reset. Another forum user confirmed that to avoid the error, the contacts for the residual moisture sensor would have to be bridged during the test procedure. So this error is extepcted (during testing) and not an issue.

I wonder how many perfectly functional devices are thrown away every year, even though they can be repaired for free with a little effort. Thanks to the forum, I learned new things during this repair, including which other components could break. Aside from saving a lot of money, it was very rewarding to fix a broken device with my own hands.
