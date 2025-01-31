+++
slug = 'shari-asl-node'
title = 'Building a SHARI AllStarLink Node'
date = 2023-09-08T12:43:00+00:00
draft = false
tags = ["AllStarLink", "Ham Radio"]
+++
After having [built an AllStarLink node for stationary operation](/allstarlink-node-build/) in the past, I decided to build a second node for mobile operation. It should be simple, inexpensive and relatively compact. Furthermore, a power supply via USB (5V DC) was a prerequisite for this project.

The choice fell on the inexpensive SHARI hats, which are based on the well-known SA818 module (hence SHARI, which stands for "SA818 Ham Allstar Radio Interface"). These can be purchased directly from China for about 55€. 

Ready built, the node looks like this:

![](/img/shari-asl-node-1.jpg)

![](/img/shari-asl-node-2.jpg)

### Hardware Modification

Additionally I ordered a SMD LFCN-490+ low pass filter, since the SHARI hats are known to generate not very clean emissions. I found this very good wiki page that describes how to add this kind of LPF to the SHARI: [wiki.fm-funknetz.de](https://wiki.fm-funknetz.de/doku.php?id=fm-funknetz:technik:shari-sa818) / [archive.org](https://web.archive.org/web/20230329192546/https://wiki.fm-funknetz.de/doku.php?id=fm-funknetz:technik:shari-sa818).

The following picture shows the naked SHARI board in unmodified state:

![](/img/shari-asl-node-3.jpg)

After opening the case, I've removed some of the protective coating between the antenna port and the SA818 module. Then I've cut a larger portion of the trace with a sharp knife:

![](/img/shari-asl-node-4.jpg)

Everything was now prepared for soldering the LPF to the PCB. Please note that the component must be placed lengthwise to the disconnected trace and the side pads of the LPF must be soldered to GND:

![](/img/shari-asl-node-5.jpg)

After this mod has been done, I've reassembled the SHARI hat and connected it to a Raspberry Pi 3 with both USB connectors. It is very important to initially connect both since one is for the USB sound card and the other one to program the SA818. During later operation, only one USB port needs to be connected.

I've then performed the following steps to install and configure ASL:

### Base System Installation

* Download the latest ASL 2.0.0 beta image from here: [http://downloads.allstarlink.org/ASL_Images_Beta/Raspberry_Pi2_3_4/](http://downloads.allstarlink.org/ASL_Images_Beta/Raspberry_Pi2_3_4/)
* Flash the image to a micro SD card
* Connect the Raspberry Pi to your local network
* Boot the Raspberry Pi
* SSH to the dynamically assigned IP address and port 222 (user: repeater, pass: allstarlink)
* Become root (sudo su -)
* Update the Linux installation: 
  * _curl -s http://apt.allstarlink.org/repos/repo_signing.key | apt-key add_
  * _apt update --allow-releaseinfo-change_
  * _apt dist-upgrade_

### SHARI Module Configuration/Programming

* Install some libraries that are needed for programming the SA818
  * _apt-get install python3-dev python3-pip_
  * _pip3 install pyserial_
* Option 1:
  * Download the SA818 programming script from [https://wiki.fm-funknetz.de/doku.php?id=fm-funknetz:technik:shari-sa818](https://wiki.fm-funknetz.de/doku.php?id=fm-funknetz:technik:shari-sa818)
  * Save the script as _~/sa818-running.py_
  * _chmod u+x ~/sa818-running.py_
  * Use your favorite editor to adapt the script to your needs (frequency, CTCSS tones)
  * Execute the script: _./sa818-running.py_
* Option 2:
  * Download and execute the following interactive Python script to program the SA818: [https://gist.github.com/g7ufo/6694bc2a1e03138f4e51064c483bec9d](https://gist.github.com/g7ufo/6694bc2a1e03138f4e51064c483bec9d)

Here's the script in case the above link doesn't work:

```
#!/usr/bin/env python3

import serial

serport = '/dev/ttyUSB0'

baud = '9600'

channelspace = '1'      # 0=12.5kHz, 1=25kHz

rxfreq = '433.6500'     # TX frequency
txfreq = rxfreq         # Same as rx freq - we work simplex

squelch = '1'           # 0-8 (0 = open)

txcxcss = '0004'        # CTCSS 77Hz
rxcxcss = '0004'        # CTCSS 77Hz
# txcxcss = rxcxcss

# txcxcss = '023N'        # CTCSS / CDCSS TX
# rxcxcss = '023N'        # CTCSS / CDCSS RX

flataudio = '1'           # switch to discriminator output and input if value = 1
bypass_lowpass = '1'      # bypass lowpass-filter if value = 1
bypass_highpass = '1'     # bypass highpass-filter if value = 1

volume = '7'              # betweeen 0..8

ser = serial.Serial(serport, baud, timeout=2)
print('Opening port: ' + ser.name)

print ('\r\nConnecting...')
ser.write(b'AT+DMOCONNECT\r\n')

output = ser.readline()
print ('reply: ' + output.decode("utf-8"))

print ('\r\nConfiguring radio...')
config = 'AT+DMOSETGROUP={},{},{},{},{},{}\r\n'.format(channelspace, txfreq, rxfreq, txcxcss, squelch, rxcxcss)
print (config)
ser.write(config.encode())
output = ser.readline()
print ('reply: ' + output.decode("utf-8"))

print ('\r\nSet filter...')
config = 'AT+SETFILTER={},{},{}\r\n'.format(flataudio, bypass_highpass, bypass_lowpass)
print(config)
ser.write(config.encode())
output = ser.readline()
print ('reply: ' + output.decode("utf-8"))

print ('\r\nSetting volume...')
config = 'AT+DMOSETVOLUME={}\r\n'.format(volume)
print(config)
ser.write(config.encode())
output = ser.readline()
print ('reply: ' + output.decode("utf-8"))

print ('\r\nSetting emission tail tone...')
ser.write(b'AT+SETTAIL=0\r\n')
output = ser.readline()
print ('reply: ' + output.decode("utf-8"))

print ('\r\nGetting Module Version...')
ser.write(b'AT+VERSION\r\n')
output = ser.readline()
print ('reply: ' + output.decode("utf-8"))

print ('\r\nGetting Settings...')
ser.write(b'AT+DMOREADGROUP\r\n')
output = ser.readline()
print ('reply: ' + output.decode("utf-8"))
```

### AllstarLink Configuration

* Execute the tool _asl-menu_ to configure your node
* Go through the menus and configure your node, start with "Run first-time menu". Follow the instructions and enter your node number etc.
* [Register the node at the AllStarLink website](https://www.allstarlink.org/portal/nodes.php)
* Enable the simple usb module by uncommenting "rxchannel=SimpleUSB/usb" in _/etc/asterisk/rpt.conf_

### Sound Card Configuration

Apart from the USB sound card settings, the node should now be functional, e.g. able to connect to other nodes. To configure the integrated sound card of the SHARI, the file _/etc/asterisk/simpleusb.conf_ has to be modified as follows:

```
; SimpleUSB configuration
[general]
[usb] ;<---- match this with your rxchannel setting in rpt.conf
eeprom = 0
hdwtype = 0
rxboost = 0
carrierfrom = usbinvert
ctcssfrom = no
deemphasis = yes
plfilter = no
rxondelay = 5
rxaudiodelay = 10
txmixa = voice
txmixb = no
txboost = 0
invertptt = 0
preemphasis = yes
duplex = 0
```
### Further Steps

* Audio levels might need to be adjusted with the tool _simpleusb-tune-menu_
* [Configure port forwarding on your router](http://wiki.allstarlink.org/wiki/ASL_FAQ#Does_app_rpt_work_through_NAT_routers.3F) if the AllStarLink node will be deployed behind NAT