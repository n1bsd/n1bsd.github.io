+++
slug = 'uv-k5-linux-firmware-programming'
title = 'QuanSheng UV-K5: Custom Firmware and Programming on Linux'
date = 2023-12-17T08:00:00+00:00
draft = false
tags = ["Ham Radio", "Linux"]
+++

**Update 2024-02-05: Added new section "Troubleshooting"**

This post is intended to help you get started with migrating the QuanSheng UV-K5 handheld radio to a custom firmware under Linux and then programming it using CHIRP.

## Firmware upgrade

I've tried several modded firmwares for the UV-K5 and finally decided to stick with the [egzumer variant](https://github.com/egzumer/uv-k5-firmware-custom). It's not only feature rich (SSB demodulation, spectrum analyzer) but also has a nice web based firmware flasher. This makes the upgrade very easy especially when running Linux. To upgrade your radio, do the following:

* turn off the HT
* connect it to the PC via the USB to Kenwood plug programming cable
* put the HT into flash mode by pressing PTT while powering it on
* open your browser (e.g. Chrome) and visit [https://github.com/egzumer/uv-k5-firmware-custom/releases](https://github.com/egzumer/uv-k5-firmware-custom/releases)
* click on the topmost link that says [🗲FLASH WITH A BROWSER🗲](https://egzumer.github.io/uvtools/?firmwareURL=https://github.com/egzumer/uv-k5-firmware-custom/releases/download/v0.20.1/egzumer_v0.20.1.packed.bin)
* click on "Flash firmware"
* select "USB-Serial Controller (ttyUSBx)"

After the radio has been successfully flashed, power it down, remove the cable and power it on again.

## Installation of CHIRP

A big advantage of the custom firmware is that the great CHIRP software can be used to program the HT. I have installed it on Debian 12 with the following commands (please replace the chirp WHL url with [the latest version available](https://trac.chirp.danplanet.com/download?stream=next)):

```
# mkdir ~/python
# cd ~/python
# python3 -m venv env
# source env/bin/activate
# wget https://trac.chirp.danplanet.com/chirp_next/next-20231213/chirp-20231213-py3-none-any.whl
# pip3 install chirp-20231213-py3-none-any.whl
```

If you don't have wxPython installed on your system, you need to do so with

```
# pip install wxpython
```

Please note that this might take a long time since the code will be compiled on your system.

Afterwards you can start chirp with the following command:

```
# chirp
```

Theoretically you can now already program your radio but you might want to be able to access the extra features of the custom firmware. To do so, the egzumer CHIRP driver needs to be installed:


* download the file [uvk5_EGZUMER.py](https://raw.githubusercontent.com/egzumer/uvk5-chirp-driver/main/uvk5_egzumer.py)
* open the "Help" menu and enable "Developer mode"
* restart CHIRP
* Open the "file" menu and select "Load module"
* Choose the previously downloaded file _uvk5_EGZUMER.py_
* When you now download the data from your HT, you will notice the new radio "Quansheng_UV_K5(egzumer)" which is available in the drop down list 
* After you have downloaded the data from the HT, open the "View" menu and enable "Show extra fields"

If you are not familiar with CHIRP and how to program a radio with it, please consult the [CHIRP Beginners Guide](https://chirp.danplanet.com/projects/chirp/wiki/Beginners_Guide).

## Troubleshooting

* If you don't see any USB devices from within your browser, please check if you've installed the browser via Snap. If you have, you need to install the browser natively as Snap prevents direct hardware access.
* If you see your USB device but don't have access, please make sure that your user is a member of the unix group "dialout". If not, add your user, logout and login again.