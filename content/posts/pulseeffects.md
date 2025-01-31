+++
slug = 'pulseeffects'
title = 'PulseEffects as Equalizer for the QO-100 Linux Transceiver'
date = 2023-11-05T17:09:00+00:00
draft = false
tags = ["Linux", "QO100"]
+++

After [migrating from Windows to Linux](/linux-migration-shack-pc/) and with this from SDR Console to the AMSAT-DL QO-100 Linux Transceiver, I had the issue that my transmitted audio was a bit weak in the treble. The solution to this was to install and configure the software equalizer _PulseEffects_ to modify my microphone audio before it gets fed into the SDR application.

### Installation

On Debian/Ubuntu Linux this application can be easily installed with the following command:

```
# sudo apt install pulseeffects
```

### Configuration

After starting PulseEffects, the following steps need to be performed:

* Click on the microphone icon on the top left (1)
* Enable the slider "SoundIO" (2)

![](/img/pulseeffects-1.png)

* Select "Equalizer" from the left menu
* Make sure the equalizer is enabled by checking the box on the left
* Click on the tools icon (3)
* Presets (4)
* Load the preset "ziyad_perfecteq" (5)

Loading this preset can be omitted but it helped me to easily reduce the number of sliders.

![](/img/pulseeffects-2.png)

* Setup your audio to your likings (6 and 7)

![](/img/pulseeffects-3.png)

Save your own preset by 
* clicking on (8),
* selecting audio input (9),
* entering a name,
* clicking the + button
* and making the settings persistent (highlighted symbol in the screenshot)

![](/img/pulseeffects-4.png)

* enable "start service at login" in the settings screen

![](/img/pulseeffects-5.png)

### Results

The following screenshot shows my signal on the waterfall with the equalizer disabled (left side) and how the signal looks like with the above settings applied (right side):

![](/img/pulseeffects-6.png)