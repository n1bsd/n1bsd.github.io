+++
slug = 'quisk-hardrock-50'
title = 'Hardrock-50 Widget for Quisk'
date = 2024-02-27T14:38:00+00:00
draft = false
tags = []
+++

My [shortwave station](/station/) consists of a Hermes Lite 2 SDR transceiver, a [Hardrock-50 power amplifier](/building-the-hardrock50/) and the marvellous [SDR software Quisk by James Ahlstrom](https://james.ahlstrom.name/quisk/). Everything works perfectly, but unfortunately the Hardrock-50 is operated relatively blindly. A lot of important information, such as the output power, the VSWR, the selected band and the operating temperature are not available remotely. The only option you have is to read out the parameters via the Hardrock-50's USB interface. [I have already tried to find a solution myself to do this remotely](/hardrock50-remote-display/), but I wasn't really satisfied with it.

By chance I found the project [hrctl by Harald Klein on Github](https://github.com/haklein/hrctl). This essentially consists of the following Python scripts:

## hrctl.py

This Python script can be executed on a single-board computer such as the Raspberry Pi, for example. It acts as a small web server that provides an API via which the above-mentioned parameters can be queried. It is also possible to use this API to send a command via USB to the Hardrock-50 to tell it to re-tune during the next transmit.

Under Debian Bookworm I had to install the following packages before running the script:

```
# apt install python3-serial python3-gevent-websocket python3-tinyrpc python3-werkzeug
```

The script can then be executed - ideally within a Tmux session - as follows:

```
# python3 ./hrctl.py
```

## hermes_widgets.py

The _hermes_widgets.py_ script is a Quisk widget that displays both the "Tune" button and the information collected by the Hardrock-50 at the bottom left of the user interface.

![screenshot](/img/quisk-hardrock-50-1.png)

To install it, place it anywhere in the file system on the system on which Quisk is to be executed. Afterwards, the following steps must then be carried out within Quisk:

* Click on "Config
* Select Hermes Lite next to "Radios" at the top
* Click on Change to the right of "Widget File Path"
* Select the script
* Restart Quisk

It may also be necessary to install Python libraries here. These are roughly the same as for the web server script.

## Conclusion

I find this solution not only elegant but also brilliant. Now all the important information about the Hardrock-50 is integrated into the Quisk GUI and I can also initiate a re-tune directly from here with a simple click.

