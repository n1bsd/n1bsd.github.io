+++
slug = 'the-arduino-enabled-washing-machine'
title = 'The Laundruino'
date = 2011-08-05T00:00:00+00:00
draft = false
tags = ["Arduino"]
+++
My washing machine is located in the basement. Unfortunately, the time data displayed on the front panel is always inaccurate. So instead of constantly running down the stairs and checking if the laundry is done, I decided to connect the washing machine to my LAN and extend it's features by a simple http server. This is what I needed for it:

  * 1 [arduino uno](http://arduino.cc/en/Main/ArduinoBoardUno)
  * 1 [ethernet shield](http://arduino.cc/en/Main/ArduinoBoardEthernet)
  * 1 opto-coupler (CNY17)
  * 1 perfboard
  * 1 resistor (150 Ohm)

In my case, getting the right signal from the washing machine was simple: It has a "Finished" LED on the front panel, so all I needed to do, was to solder it out and replace it with a two-core wire.


![](/img/the-arduino-enabled-washing-machine-1.jpg)


![](/img/the-arduino-enabled-washing-machine-2.jpg)


![](/img/the-arduino-enabled-washing-machine-3.jpg)



To connect my washing machine with the arduino, I built a minimal shield with a prefboard and a CNY17. Here's how everything is put together:


![](/img/the-arduino-enabled-washing-machine-4.jpg)


![](/img/the-arduino-enabled-washing-machine-5.jpg)


![](/img/the-arduino-enabled-washing-machine-6.jpg)


Now I have a LOW signal on pin A2 (digital mode) when the laundry is done and a HIGH signal if the machine is running or turned off. In my case, a loop of 100ms duration is needed every time to watch the signal because I don't get a steady voltage here (I guess the front panel LEDs might be multiplexed). If  the signal becomes LOW once in this time window, the washing machine has finished and the LED would have turned on.

![](/img/the-arduino-enabled-washing-machine-7.jpg)


Screenshots:


![](/img/the-arduino-enabled-washing-machine-8.jpg)


![](/img/the-arduino-enabled-washing-machine-9.jpg)




You can download the code here: [launduino.tar.gz](/files/launduino.tar.gz)
