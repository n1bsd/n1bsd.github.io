+++
slug = 'plutoplus-firmware'
title = 'Pluto+ Firmware Update'
date = 2023-06-26T20:12:27.370023+00:00
draft = false
tags = ["Firmware", "Pluto+", "QO100"]
+++
This post describes how I update my Pluto+ SDR transceiver:

Download the latest firmware from [https://github.com/DeonMarais64/PlutoPlusSDR-FW/releases](https://github.com/DeonMarais64/PlutoPlusSDR-FW/releases). At the time of writing this article this was version [v0.37-dirty](https://github.com/DeonMarais64/PlutoPlusSDR-FW/releases/download/v0.37%2B_datv_packages/plutosdr-fw-v0.37-dirty.zip).

Preconditions / assumptions:
  * Filename: plutosdr-fw-v0.37-dirty.zip
  * IP of the Pluto+: 192.168.88.99
  * Local Windows user: abcd
  * Local OS: Windows 11

We can now either copy this file to the SD card of the Pluto+ or use a scp client to transfer the file via the network. This post describes how to perform the update via SCP.

We can now open CMD.exe and transfer the file via scp:

```bat
C:\temp>scp c:\users\micha\Downloads\plutosdr-fw-v0.37-dirty.zip root@192.168.88.99:/root/
root@192.168.88.99's password:
plutosdr-fw-v0.37-dirty.zip                                                           100%   44MB  12.3MB/s   00:03
```
The Pluto's default password for the user "root" is "analog".

After the file has been transferred, SSH into the Pluto+:

```bat
C:\temp>ssh root@192.168.88.99
root@192.168.88.99's password:
Welcome to:
______ _       _              _________________
| ___ \ |     | |         _  /  ___|  _  \ ___ \
| |_/ / |_   _| |_ ___  _| |_\ `--.| | | | |_/ /
|  __/| | | | | __/ _ \|_   _|`--. \ | | |    /
| |   | | |_| | || (_) | |_| /\__/ / |/ /| |\ \
\_|   |_|\__,_|\__\___/      \____/|___/ \_| \_|

v0.35-1-g714c-dirty
https://github.com/DeonMarais64/PlutoPlusSDR-FW
#
```
Now unzip the firmware package:

```bat
# unzip plutosdr-fw-v0.37-dirty.zip
Archive:  plutosdr-fw-v0.37-dirty.zip
  inflating: pluto.dfu
  inflating: uboot-env.dfu
  inflating: pluto.frm
  inflating: boot.dfu
  inflating: boot.frm
# 
```

The firmware update process can now be started as following:

```bat
# update_frm.sh ./pluto.frm
356+1 records in
356+1 records out
Done
#
```

Please be patient, the update might take a while. After the update has been done, we now reboot the Pluto+:

```bat
# reboot
```

After waiting a bit for the system to restart, we can now SSH into the Pluto+ again to confirm the successful update:

```bat
C:\temp>ssh root@192.168.88.99
root@192.168.88.99's password:
Welcome to:
______ _       _              _________________
| ___ \ |     | |         _  /  ___|  _  \ ___ \
| |_/ / |_   _| |_ ___  _| |_\ `--.| | | | |_/ /
|  __/| | | | | __/ _ \|_   _|`--. \ | | |    /
| |   | | |_| | || (_) | |_| /\__/ / |/ /| |\ \
\_|   |_|\__,_|\__\___/      \____/|___/ \_| \_|

v0.37-dirty
https://github.com/DeonMarais64/PlutoPlusSDR-FW
#
```
