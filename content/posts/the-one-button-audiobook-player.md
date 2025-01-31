+++
slug = 'the-one-button-audiobook-player'
title = 'The One Button Audiobook Player'
date = 2012-10-29T00:00:00+00:00
draft = false
tags = ["Hardware", "Python", "Raspberry Pi"]
+++
This little Raspberry Pi based project is a gift for my wife's grandmother for her 90th birthday. Being visually impaired, she is hard to entertain but loves to listen to audiobooks. The problem is, that she isn't able to handle a ghetto blaster or MP3 player.

The solution to this problem was &#8211; tadaaaah &#8211; a one button audiobook player 🙂

It basically consists of:

  * 1 Raspberry Pi
  * 1 [ModMyPi enclosure](https://www.modmypi.com/)
  * 1 button
  * 2 resistors (330 Ohm, 10 Kilo-Ohm)
  * 1 blue LED
  * 1 (slow) 8GB SD-Card
  * some wire
  * a pair of speakers

The following software has been used:

  * Raspbian minimal image ([http://www.linuxsystems.it/2012/06/raspbian-wheezy-armhf-raspberry-pi-minimal-image](http://www.linuxsystems.it/2012/06/raspbian-wheezy-armhf-raspberry-pi-minimal-image/))
  * mpd (music player daemon)
  * mpc
  * mpd-python
  * pyudev (for USB access)
  * a self-written python script

The features are the following:

  * **always on:** When you power on the raspberry, it will boot up and start the python script with the audio book in pause
  * **one button usage:** The button pauses and unpauses the audio book or goes back one track when you press the button longer than 4 seconds
  * **remembers position:** It will always remember the last played position
  * **only one audiobook:** There will always be only one audio book on the Raspberry
  * **easy audio book deployment:** When you plug in a USB thumb drive with a special name/label, the Raspberry will stop playing, mount the thumb drive, deletes the old audio book, copies the new one, rebuilds the playlist and &#8211; after unplugging the thumb drive &#8211; starts the new audiobook in pause mode
  * **multi format:** Since it uses mpd, the player supports  Ogg Vorbis, FLAC, OggFLAC, MP2, MP3, MP4/AAC, MOD, Musepack and wave

Some pics:

![](/img/the-one-button-audiobook-player-1.jpg)


![](/img/the-one-button-audiobook-player-2.jpg)


If you like to build your own one button audio book player, here are the super simple schematics:


![](/img/the-one-button-audiobook-player-3.jpg)


And last but not least &#8211; the python script. The code might be crappy, please comment if you have improvements (especially regarding loadMusic). You can find it here:

[theonebuttonaudiobookplayer.tar.gz](/files/theonebuttonaudiobookplayer.tar.gz)

**Update (2013-11-26)**

Here's what Russel wrote in a comment to this post:

    I just completed building this and have some addendum notes adding more details:
    Install the following packages:
    sudo apt-get install mpd
    sudo apt-get install mpc
    sudo apt-get install python-mpd
    sudo apt-get install python-pyudev

    (below assumes using defaults for /etc/mpd.conf)
    sudo mkdir -p /music/usb
    sudo ln -s /var/lib/mpd /music/mpd
    sudo ln -s /var/lib/mpd/music /music/mp3

    Copy the tobabp.py script to /home/pi
    nano /home/pi/tobabp.py

    Change these in the script or flip the connections in wiring diagram.
    BUTTON = 17
    LED = 24

    Testing

    Rename a USB stick to “1GB”
    Copy 1 MP3 onto the stick

    Insert the stick into pi
    sudo mount /dev/sda1 /music/usb
    sudo /etc/init.d/mpd stop
    sudo rm /music/mp3/*
    sudo cp /music/usb/* /music/mp3/
    sudo umount /music/usb

    Remove the USB stick
    sudo rm /music/mpd/tag_cache
    sudo /etc/init.d/mpd start
    mpc clear
    mpc ls
    mpc ls | mpc add
    sudo /etc/init.d/mpd restart
    mpc play

    Plug in earphones
    You should hear audio

    Next try the python script:
    sudo python /home/pi/tobabp.py

    Insert USB stick
    the LED should flash and the USB file copy to /music/mp3/
    the LED should flash again. Remove the Stick and LED flashes again.
    Press button to start playing
    Press button again to stop
    Press & hold button to rewind to beginning.
    sudo crontab -e
    Add following line run at startup
    @reboot python /home/pi/tobabp.py &
    sudo reboot

    Then retest again to be sure all is well.
