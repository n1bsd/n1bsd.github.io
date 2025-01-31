+++
slug = 'aprs-tx-igate-with-aprx'
title = 'APRS TX i-Gate with APRX and the Universal Radio Controller'
date = 2024-05-04T00:00:00+00:00
draft = false
tags = ["APRS", "Ham Radio"]
+++

After I recently discovered APRS for myself, I realized that I could not establish radio contact with the surrounding APRS digipeaters in and around the house with the HT. Connected to the X200 on the roof of the house, however, there were no problems. This immediately gave me the idea of operating my own digipeater. For a first test, I then connected my picoAPRS v4 to the X200 and configured the device as a fill-in digipeater. The transmissions from my handheld radio were picked up and retransmitted by the picoAPRS without any problems to the next public I-Gate, but APRS messages sent to me never reached me.

So I went straight to the next experiment and ordered a [G1LRO Universal Radio Controller](https://g1lro.uk/?p=635) (URC) including the corresponding APRS board, assembled it and put it into operation together with a Quansheng UV-K5. With the VP-Digi software included with the URC, this was quick and easy, but the result was exactly the same as with the picoAPRS. In the case of the URC, thanks to the monitor functionality, which allows me to observe the received/transmitted packets via the serial interface, I was able to see that the APRS messages sent to me did not even reach the digipeater. So it was not picoAPRS or the Universal Radio Controller failing.

The new idea was to build a TX I-Gate. Out of the box, however, this is not possible with the URC as it has no network connectivity. I found a disused Raspberry Pi Zero in the basement, on which I first installed Debian and then the APRX-2.9 package, which is fortunately in the Debian repository.

The Universal Radio Controller had to be reconfigured beforehand. To do this, I connected to it via the serial interface and deactivated both the beaconing and the digipeater functionality. The final configuration of the URC looks like this:

```
Modem: AFSK Bell 202 1200 Bd 1200/2200 Hz
Callsign: N0CALL-1
Destination: APNV01
TXDelay (ms): 300
TXTail (ms): 30
Quiet time (ms): 300
USB: default mode: KISS
UART1: 9600 baud, default mode: KISS
UART2: 9600 baud, default mode: KISS
DAC type: PWM
Flat audio input: no
Beacon 0: Off, Iv: 10, Dl: 0, WIDE2-2, =<REDACTED>N/<REDACTED>E#Digipeater
Beacon 1: Off, Iv: 0, Dl: 0, no path,
Beacon 2: Off, Iv: 0, Dl: 0, no path,
Beacon 3: Off, Iv: 0, Dl: 0, no path,
Beacon 4: Off, Iv: 0, Dl: 0, no path,
Beacon 5: Off, Iv: 0, Dl: 0, no path,
Beacon 6: Off, Iv: 0, Dl: 0, no path,
Beacon 7: Off, Iv: 0, Dl: 0, no path,
Digipeater: Off
Alias WIDE: On, max: 2, rep: 3, traced, viscous-delay, unfiltered
Alias : Off, max: 0, rep: 0, untraced, unfiltered
Alias : Off, max: 0, rep: 0, untraced, unfiltered
Alias : Off, max: 0, rep: 0, untraced, unfiltered
Alias -0: Off, untraced, unfiltered
Alias -0: Off, untraced, unfiltered
Alias -0: Off, untraced, unfiltered
Alias -0: Off, untraced, unfiltered
Anti-duplicate buffer hold time (s): 30
Callsign filter type: blacklist
Callsign filter list: 0 entries
KISS monitor: Off
Allow non-APRS frames: Off
FX.25 protocol: Off
FX.25 TX: Off
```

Admittedly, it took me far too long to understand the configuration of a TX I-Gate with APRX and I finally found the following configuration that worked for me by trial and error:

```
mycall  <N0CALL-1>
myloc lat <REDACTED>N lon <REDACTED>E

<aprsis>
  login              $mycall
  passcode           <REDACTED>
  server             rotate.aprs2.net
  heartbeat-timeout  0         # Disabler of heartbeat timeout
  filter             "m/100"   # positions within 100 km from my location
</aprsis>

<logging>
  pidfile            /var/run/aprx.pid
  rflog              /var/log/aprx/aprx-rf.log
  aprxlog            /var/log/aprx/aprx.log
</logging>

<interface>
  serial-device      /dev/ttyACM0 9600 8n1 KISS
  callsign           $mycall
  alias	             RELAY,WIDE,TRACE
  tx-ok              true    # transmitter enable defaults to false
  telem-to-is        true # set to 'false' to disable
</interface>

<beacon>
  beaconmode         radio
  cycle-size         20m
  beacon             symbol "I#" $myloc comment "Tx iGate + Digipeater"
</beacon>

<digipeater>
  transmitter        $mycall
  <source>
    source           $mycall
    relay-type       digipeated # default mode is "digipeated"
  </source>
  <source>
    source           APRSIS
    relay-type       third-party  # Must define this for APRSIS source!
    viscous-delay    5
    ratelimit        60 120  
    filter           t/m
    via-path         WIDE1-1
    msg-path         WIDE1-1
  </source>
</digipeater>
```

Here's an overview of all components used in this setup:

* Raspberry Pi Zero 1.1 with Debian and APRX installed
* [Universal Radio Controller](https://g1lro.uk/?p=635) with APRS board directly connected to the left micro USB port of the Raspberry Pi Zero
* Quansheng UV-K5 HT set to the local APRS frequency 144.800 connected to the HT 2.5 and HT3.5 ports with 2 audio cables

I've modified the HT by replacing the battery with a 5V to 8V converter directly soldered to the battery pins:

![aprs-tx-igate-with-aprx](/img/aprs-tx-igate-with-aprx-1.jpg)

The device in all its glory:

![aprs-tx-igate-with-aprx](/img/aprs-tx-igate-with-aprx-2.jpg)

I'm aware that the whole setup doesn't look very professional, but it works.

With this configuration, the I-Gate works in such a way that I can send and receive APRS messages as well as positions with my handheld radio. I assume that it is not optimal and would be happy to receive suggestions for improvement [via email](/about/).
