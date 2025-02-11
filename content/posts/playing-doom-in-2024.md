+++
slug = 'playing-doom-in-2024'
title = 'Playing Doom in 2024 (not only) on Linux'
date = 2024-07-27T11:43:00+00:00
draft = false
tags = ["Doom", "Games"]
+++
Besides amateur radio, I have another passion: Doom. Not the modern Doom variants, however, but the classic Doom or Doom II from the 90s (when I write about Doom in general in the rest of this article, I mean Doom and Doom II). There is hardly any other game that has such an incredibly persistent and active community as Doom. It delivers a continuous stream of total conversions, WADs, megawads and further developments of the numerous source ports.

The terms just mentioned may need some explanation:

* __Source Port:__ A Doom source port is a modernized version of the original Doom engine, modified to run on modern operating systems offering enhancements and new features. They are based on the original Doom source code, which was released by id Software in 1997.
* __Total Conversion:__ An expansion of Doom in the form of a WAD or PK3, which is effectively a new game based on the Doom engine: New weapons, enemies, levels, textures, items, etc.
* __WAD__: This file name extension stands for ‘Where's All the Data?’. These files contain the Doom game files and are divided into IWADs and PWADs:
  * __IWAD (Internal WAD):__ the WAD on which the game is based (e.g. DOOM.WAD or DOOM2.WAD), which is required for the game to run
  * __PWAD (Patch WAD):__ optional WAD that is applied like a patch and overwrites parts of the IWAD, e.g. levels, monsters etc.
* __Megawad__: like a WAD but includes 15 or more new levels, some of them aim to deliver 32 maps like the original game
* __PK3__: Not the PK3 you might expect if you are a Quake player. It's actually just a renamed zip file containing Doom game content.

## Choosing a Source Port

There are many source ports to chose from:

* __[GZDoom](https://zdoom.org):__ One of the most popular source ports, known for its advanced rendering capabilities, including OpenGL support, dynamic lighting, and scripting enhancements with ZScript.
* __[Zandronum](https://zandronum.com/):__ Focuses on multiplayer gameplay, providing extensive online play features, and is based on GZDoom.
* __[Chocolate Doom](https://www.chocolate-doom.org/):__ Strives to closely replicate the original Doom experience, maintaining the look, feel, and behavior of the original game.
* __[DSDA](https://doomwiki.org/wiki/DSDA-Doom):__ Offers a balance between compatibility with the original game and enhancements, often used for speedrunning due to its demo recording and playback capabilities. PrBoom+ based.

I would recommend to start with the [zDoom based source port GZDoom](https://zdoom.org). Later on you might want to try other source ports, e.g. because a specific megawad you want to play requires a prboom based port instead of a zDoom based one. GZDoom is nice and easy to use but also has its downsides. It eats quite an amount of resources and can actually be laggy even on modest modern systems when there are many monsters nearby.

Download GZDoom [here](https://zdoom.org/downloads) and install it. Before we execute it, we need the original Doom WADs.

## Obtaining the Doom IWADs

There are many ways to obtain the required IWADs. One legal option is to buy them from Steam or GOG. As GOG sells only DRM free games, I consider them being the good guys so I recommend buying the games from them:

* [Doom](https://www.gog.com/en/game/doom_1993)
* [Doom II](https://www.gog.com/en/game/doom_ii)

The downloaded games are Windows installer files which we first need to install with wine in order to obtain the IWADs:

```
odie@debian:~$ wine Downloads/downloaded_installer_file.exe
```

You will find the needed files here (with Doom II as an example):

```
odie@debian:~/.wine/drive_c/GOG Games/DOOM 2/doom2$ ls
DEFAULT.CFG  DM.CFG  dm.dat  DM.EXE  DOOM2.EXE  DOOM2.WAD  IPXSETUP.EXE  MODEM.CFG  MODEM.NUM  MODEM.STR  MOUSE.CFG  SERSETUP.EXE  SETUP.EXE
```

I've created a directory named _wads_ in my home directory and copied the required WADs there:

```
odie@debian:~/.wine/drive_c/GOG Games/DOOM 2/doom2$ cp *.WAD ~/wads/
```

Repeat these steps for any GZDoom supported game.

## Playing Doom

To play Doom, _cd_ into the wads directory and execute GZdoom:

```
odie@debian:~$ cd wads
odie@debian:~/wads$ gzdoom 
GZDoom g4.12.2 - 2024-04-26 15:12:47 -0400 - SDL version
Compiled on Apr 30 2024

OS: Debian GNU/Linux 12 (bookworm), Linux 6.1.0-23-amd64 on x86_64
GZDoom version g4.12.2
```

You will now be greeted with the GZDoom launcher. The "Game" tab should list all Doom games you have copied to your _wads_ directory. GZDoom (and most other ports) are not limited to Doom. They are also capable of playing other Doom engine based games like Heretic or Hexen.

Select the game you want and switch to the "Options" tab. You might want to disable "Lights" and "Brightmaps" under "Extra Graphics" as even modern low-spec systems might become laggy with these options enabled.

Finally, click "Play Game".

## Configuration

In the start screen press "Esc", select options, then "Full Options". Now you might want to configure the following:

* "Set video mode" to pick a video resolution and to enable full screen
* "Customize Controls" to set everything to your liking. I prefer "WASD" mode and the default key binding
* "Mouse Options" to set the sensitivity to your liking

Now it's time to play!

## Recommended Playing Order

Here is my recommended playing order for someone playing Doom the first time since the 90s or at all:

* Doom Engine
  * The Ultimate Doom (DOOM.WAD)
  * [Sigil](https://romero.com/sigil)
  * [Sigil II](https://romero.com/sigil)
* Doom II Engine
  * Doom II Hell on Earth (DOOM2.WAD)
  * Final Doom: TNT - Evilution (TNT.WAD)
  * Final Doom: Plutonia Experiment (PLUTONIA.WAD)

## What's next?

Welcome to the endless rabbit hole of Doom! You most likely won't live long enough to play all available maps and total conversions. All WADs I've played and what I think about them can be found [here](/doom-wad-log/).

To play Doom with such a (mega)wad, simply add the name of the wad as an option when starting GZDoom:

```
odie@debian:~$ cd wads
odie@debian:~/wads$ gzdoom SIGIL_v1_21.wad 
GZDoom g4.12.2 - 2024-04-26 15:12:47 -0400 - SDL version
Compiled on Apr 30 2024

OS: Debian GNU/Linux 12 (bookworm), Linux 6.1.0-23-amd64 on x86_64
GZDoom version g4.12.2
W_Init: Init WADfiles.
adding /opt/gzdoom/gzdoom.pk3, 679 lumps
adding /opt/gzdoom/game_support.pk3, 3308 lumps
adding ./DOOM.WAD, 2306 lumps
adding /opt/gzdoom/game_widescreen_gfx.pk3, 214 lumps
adding SIGIL_v1_21.wad, 146 lumps
```

Another alternative, which is a standalone game that unfortunately only runs on Windows, is Doom 64, which is a Doom game first released on the Nintendo 64 offering completely new, exclusive levels and is definitely worth playing. It can also [be purchased from GOG](https://www.gog.com/en/game/doom_64). 

## Where to find WADs

A very good start to find some selected WADS is this forum thread on doomworld.com: [The ULTIMATE Master WAD Guide](https://www.doomworld.com/forum/topic/118178-the-ultimate-master-wad-guide/).

Then there are [the yearly Cacowards](https://www.doomworld.com/cacowards/), where each year's top releases get an award. You can propably not go wrong with these.

General info on Doom (actually ALL info you want) can be found in the [Doomwiki](https://doomwiki.org/).

## Multiplayer

I have only recently started playing Doom multiplayer, so I have little experience to share. I will expand this part of the article in the future. But here are a few helpful links:

* [https://www.doomworld.com/forum/topic/69270-how-to-play-doom-multiplayer-tutorial-info/](https://www.doomworld.com/forum/topic/69270-how-to-play-doom-multiplayer-tutorial-info/)
* [https://doomwiki.org/wiki/How_to_play_Doom_online_multiplayer](https://doomwiki.org/wiki/How_to_play_Doom_online_multiplayer)
* [https://wiki.zandronum.com/Playing_online](https://wiki.zandronum.com/Playing_online)
