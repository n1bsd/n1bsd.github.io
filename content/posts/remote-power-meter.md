+++
slug = 'remote-power-meter'
title = 'Remote Power/SWR Meter'
date = 2023-07-26T00:00:00+00:00
draft = false
tags = ["Arduino", "ESP32", "Ham Radio", "Remote"]
+++
This WT32/ESP32 based project, combined with a directional coupler setup, allows you to remotely monitor the output power and SWR of your station via a web browser.

It reads two voltages which are supplied by the directional couplers. From these, the respective power is calculated with the help of a calibration data table to be created by the user.

Credits for helping me with the directional coupler setup, software testing and feature ideas go to [Matthias DD1US](http://dd1us.de/).

![](/img/remote-power-meter-1.png)

## Preconditions

### Hardware

The following hardware is needed for this project:

* [wt32-eth01 development board](http://en.wireless-tag.com/product-item-2.html)
* USB to Serial adapter (FTDI)
* directional coupler setup which outputs one voltage (0 to 3.3V) each for the forward and return power. [Matthias DD1US](http://dd1us.de/) uses the following components in his implementations:
  * Ericsson coupler + AD8318 detector
  * Narda coupler + AD8313 detector

My implementation consists of the directional coupler, which i took from an old swr/power meter:

![](/img/remote-power-meter-2.jpg)

![](/img/remote-power-meter-3.jpg)

After removing it from the enclose, it needed some shielding:

![](/img/remote-power-meter-4.jpg)

It fits perfectly into my favorite 5€ project box (Donau Elektronik - KGB15 Euro Box klein, Blau, 95x135x45) with custom 3D printed face plates:

![](/img/remote-power-meter-5.jpg)

The wiring has been done according the diagram further below.

![](/img/remote-power-meter-6.jpg)


### Software / Libraries

* Arduino IDE
* Project code: [wt32powermeter.tar.gz](/files/wt32powermeter.tar.gz)
* Libraries:
  * WebServer_WT32_ETH01: [https://github.com/khoih-prog/WebServer_WT32_ETH01](https://github.com/khoih-prog/WebServer_WT32_ETH01)
  * DallasTemperature
  * OneWire

## Downloading and Configuring the Arduino IDE

The following steps are needed to be able to compile and upload the code:

* Download and install the Arduino IDE 2.1: [https://wiki-content.arduino.cc/en/software](https://wiki-content.arduino.cc/en/software)
* Follow this guide to install the ESP32 board definitions: [https://randomnerdtutorials.com/installing-esp32-arduino-ide-2-0/](https://randomnerdtutorials.com/installing-esp32-arduino-ide-2-0/)
* Select the correct board inside the Arduino IDE: Tools -> Board -> esp32 -> ESP32 Dev Module
* Install all needed libraries:
  * Tools -> Manage Libraries -> Search for "WebServer_WT32_ETH01" -> Install

## Downloading the Software

* Download the code: [wt32powermeter.tar.gz](/files/wt32powermeter.tar.gz)
* Unzip the code to _c:\users\<username>\Documents\Arduino_:

![](/img/remote-power-meter-7.png)

* Open the file _wt32powermeter.ino_ inside the Arduino IDE or double-click the file

## Programming the Board

* Connect the board with a USB to serial adapter as shown in the following picture:

![](/img/remote-power-meter-8.png)

* Select the correct COM Port inside the Arduino IDE: Tools -> Port -> Select Port

* Click "Upload" (top left corner, Arrow pointing to the right)

## Connecting the Board to the Directional Coupler

![](/img/remote-power-meter-9.png)

Please note that the two pins _IO0_ and _GND_ only need to be bridged during programming. Open the bridge again after programming.

## Configuration

Please adapt the following code blocks in the file _wt32powermeter.ino_ to your needs:

### Network Configuration

```
ETH.begin(ETH_PHY_ADDR, ETH_PHY_POWER);

// Static IP, leave without this line to get IP via DHCP
//ETH.config(myIP, myGW, mySN, myDNS);

WT32_ETH01_waitForConnect();
```
By default, wt32powermeter is configured to dynamically get assigned an IP address via DHCP. If you are happy with this, no actions are needed.
If not, uncomment the line _ETH.config(myIP, myGW, mySN, myDNS);_ by removing the two slashes at the beginning of the line to have a static IP configuration assigned.

Please define the desired network configuration in the following code block:

```
// Select the IP address according to your local network
IPAddress myIP(192, 168, 1, 100);
IPAddress myGW(192, 168, 1, 1);
IPAddress mySN(255, 255, 255, 0);
IPAddress myDNS(192, 168, 1, 1);

```

### Selectable Bands

To add or remove bands, find the following code block:

```
String band = "";
String default_band = "70cm";
String band_fwd = band + "_fwd";
String band_ref = band + "_ref";
String band_list[] = { "1.25cm", "3cm", "6cm", "9cm", "13cm", "23cm", "70cm", "2m", "HF" };
```

Add/remove bands from the _band_list[]_ variable and set your desired default band with _default_band_.

## Accessing the Web Interface

Open your favorite browser and navigate to _http://YOUR_IP_ADDRESS_, for example _http://192.168.1.100_.

The IP address is either the address you've defined above or a dynamically assiged address. To find out the latter, you might want to log in to your router and look for a dashboard that shows all current connected network devices.

## Usage

The first step is to configure your connected directonal couplers by clicking "Configuration" in left side of the page footer.

![](/img/remote-power-meter-10.png)

### Calibration Data

First, select the band you want to configure via the top right dropdown box labeled "Band".

Please ignore the actual values of the calibration data. These values have been used for debugging purposes. You need to find your own values for your setup. Enter here the mV:dBm value pairs for FWD and RED and click "Save Calibration Data".

You can enter your data in any line, there is no need to manually sort the value pairs. After saving the data, it will be automatically sorted and correctly displayed.

Here is how peformed the calibration in my environment:

* Connected the components as folowed: IC-7300 -> Remote Power Meter -> known good power meter -> dummy load
* Set the band to 20m, mode FM
* Set power to 1W, pushed PTT, read the measured power of the known good power meter and the voltage measured by the Remote Power Meter and took notes for the FWD calibration table
* repeated the previous step until I reached max power
* put out the Remote Power Meter, reversed it and put it back into the chain of devices
* Set power to 1W, pushed PTT, read the measured power of the known good power meter and the voltage measured by the Remote Power Meter and took notes for the REF calibration table
* calculated the dBm values for each entry in my notes and replaced every line with <voltage>:<calculated dBm from W>
* pasted both tables into the Remote Power Meter's config page

### General Configuration Items

The following general configuration items are available to customize the application:

* _Show voltage in mV (yes/no)_: Enables or disables the display of the measured voltage
* _Show power level in dBm (yes/no)_: Enables or disables the display of the measured power level in dBm
* _Show power in Watt (yes/no)_: Enables or disables the display of the measured power in Watt
* _VSWR threshold that triggers a warning (e.g. 3)_: any calculated value exceeding the configured value will result in a visual and optionally acoustic warning
* _Beep if VSWR threshold is exceeded (yes/no)_: beeps if the above configured threshold is exceeded. Works only in some browsers.
* _Name of the antenna: Freely definable name of this band's antenna
* _Max. FWD power displayed by LED bar graph in W (e.g. 100)_: Sets the upper limit of the LED bar graph for the calculated FWD power
* _Max. REF power displayed by LED bar graph in W (e.g. 100)_: Sets the upper limit of the LED bar graph for the calculated REF power
* _Max. VSWR displayed by LED bar graph (e.g. 3)_: Sets the upper limit of the LED bar graph for the VSWR
* _Show LED graph for FWD power (yes/no)_: enables or disables the VU-meter style LED grapg for the FWD power
* _Show LED graph for REF power (yes/no)_: enables or disables the VU-meter style LED grapg for the REF power
* _Show LED graph for VSWR (yes/no)_: enables or disables the VU-meter style LED grapg for the VSWR
* _Cable loss in db (e.g. 3)_: Sets the cable loss of your system which then will be considered during calculations

After you are done with the desired changes, click "Save Configuration".

## Warning / Disclaimer

This software contains no security mechanisms at all. There is no input/output sanitization/validation in place. Furthermore there is no authentication or authorization mechanism implemented. Any person/system inside the network is able to access the application, read and modify configuration items, gather information about monitored devices.

DO NOT MAKE THE APPLICATION PUBLICLY AVAILABLE! DO NOT EXPOSE THIS TO THE INTERNET.

If you feel uncomfortable using the software as it is, you are more than welcome to contribute, improve, submit PRs etc.
