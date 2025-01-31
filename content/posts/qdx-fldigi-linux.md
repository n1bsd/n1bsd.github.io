+++
slug = 'qdx-fldigi-linux'
title = 'QDX and Fldigi on Debian Linux'
date = 2024-08-09T16:07:00+00:00
draft = false
tags = ["Ham Radio", "QDX", "Linux", "Fldigi", "QRP"]
+++
The following instructions describe how to configure Fldigi for operation with the [QRP Labs QDX transceiver](/qubedx/) on Debian Linux 12 / Raspberry Pi OS (64bit).

In the configuration menu, click on _Soundcard -> Devices_ and select _PulseAudio_:

![](/img/qdx-fldigi-linux-01.png)

Afterwards, click on _Rig Control -> Hamlib_ and configure it as follows:

![](/img/qdx-fldigi-linux-02.png)

With these few settings it almost worked for me - but only almost. The QDX switched to transmit mode, but no signal was emitted and the red LED flashed twice continuously. This indicated that the audio signal was too low. 

To fix this, open the _PulseAudio Volume Control_ tool and set the volume under _Output Devices_ to 11dB:

![](/img/qdx-fldigi-linux-03.png)

If _PulseAudio Volume Control_ is not installed on the system, this can be done using the following command:

```
# sudo apt install pavucontrol
```
