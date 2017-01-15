---
layout: post
title: "Wolfenstein-3D on RetroPie"
date: 2017-01-15 13:13:15 +0100
comments: true
categories:
---

The last game I ever played was Castle Wolfenstein 3D, released in 1992
for MS-DOS, made by the now legendary id Software.  OK, admittedly I've
played other games since then, but I've never again been so manic about
a game since Wolfenstein.

This post is about how to build, install and set up Wolfenstein 3D on a
Raspberry Pi running RetroPie ... in a Picade :-)

<!-- more -->

First, you need the original WAD files for the game.  It's really worth
investing the money if you don't already have a copy.  Mount the CD, or
the CD image to `/mnt`, the files are located in:

    /mnt/Install/data/WOLF3D/

The `.wl6` files should be installed in `/usr/share/games/wolf3d/`, in
*lower-case*.  Use the following command to rename them from upper-case:

    rename 'y/A-Z/a-z/' *

Second, download the game source from GitHub:

    mkdir Source && cd Source/
    git clone https://github.com/mozzwald/wolf4sdl.git
    cd wolf4sdl/

To build the game you need to install the SDL libraries, in particular
the `-dev` packages for both SDL and the SDL mixer:

    sudo apt install libsdl*-dev 

Now, build the game:

    PREFIX=/usr make

To start the game in RetroPie (Emulation Station), create the following
script:

    cd ~/RetroPie/roms/ports
    vim Wolfenstein-3D.sh

Copy and paste the following lines:

    #!/bin/bash
    cd /home/pi/Source/wolf4sdl
    /opt/retropie/supplementary/runcommand/runcommand.sh 0 "./wolf3d --res 640 480"

Remember to set the executable flag on the script before restarting the
Emulation Station GUI:

    chmod +x Wolfenstein-3D.sh

Done.  Restart and enjoy the game on your Picade like I do :-)

<!--
  -- Local Variables:
  -- mode: markdown
  -- End:
  -->
