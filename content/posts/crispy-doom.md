+++
slug = 'crispy-doom'
title = "Crispy Doom"
date = 2025-02-12T08:00:00+00:00
draft = false
tags = ["Doom"]
+++

I've recently moved away from [GZDoom](https://zdoom.org/downloads) and picked [Crispy Doom](https://doomwiki.org/wiki/Crispy_Doom) as my main Doom source port.

My reasons for this are performance problems with GZDoom (believe it or not) and the desire for a more original gaming experience. 

Crispy Doom is a fork of [Chocolate Doom](https://github.com/chocolate-doom/chocolate-doom) (which is a modern source port aiming to be as vanilla as possible) and offers in addition to the vanilla gaming experience a few amenities, such as modern display resolutions and uncapped rendering framerate.

I play Crispy Doom with the original software renderer, widescreen resolution, jumping disabled, a nice crosshair for better aiming and control it with WASD plus mouse - but only on the X-axis, so without mouse look or vertical look/aiming.

![screenshot of Crispy Doom](/img/crispy-doom-01.jpg)

I still [recommend beginners to start with GZDoom](/playing-doom-in-2024/), but also to look at other source ports after a while.

For a quick start, here is the command line on how to start the one map PWAD ACHERON.WAD for Doom 1 that occupies the map slot E1M3: 

```
user@debian:~/wads$ crispy-doom -iwad DOOM.WAD -merge ACHERON.WAD -warp 13
```
