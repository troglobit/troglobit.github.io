---
layout: post
title: "Wolfenstein-3D on RetroPie"
date: 2017-01-15 13:13:15 +0100
comments: true
categories:
---

The last game I ever played was [Castle Wolfenstein 3D][1], released in
1992 for MS-DOS, made by the now legendary id Software.  OK, admittedly
I've played other games since then, but I've never again been so manic
about a game since Wolfenstein.

<img style="width:50%;height:auto;" class="center" src="/images/wolf3d.png">

This post is about how to build, install and set up Wolfenstein 3D on a
Raspberry Pi running [RetroPie][2] ... on a [Picade][3] :-)

<!-- more -->

You need the original WAD files to play the game.  It's really worth
investing the money if you don't already have a copy.  Mount the CD, or
the CD image to `/mnt` on your Linux PC, the files are located in:

    mkdir ~/wolf3d
    cd /mnt/Install/data/WOLF3D/
    cp *.WL6 ~/wolf3d/

The `.wl6` files must be installed in `/usr/local/share/games/wolf3d/`
on the Raspberry Pi, with names in *lower-case*.  Use the following
command to rename them from upper-case:

    cd
    rename 'y/A-Z/a-z/' wolf3d/*

Transfer the files to the Pi using scp, or similar:

    scp -r wolf3d pi@raspberrypi.local:.

On the Rasberry Pi:

    sudo mv ~/wolf3d /usr/local/share/games/

Now, download the game source from GitHub:

    git clone https://github.com/mozzwald/wolf4sdl.git
    cd wolf4sdl/

To build the game you need to install the SDL libraries, in particular
the `-dev` packages for both SDL and the SDL mixer:

    sudo apt install libsdl*-dev 

You may need to edit the file `version.h`, depending on what version of
the original game you have.  When done, build the game:

    make

To start Wolfenstein in RetroPie (Emulation Station), create the
following script and it will appear in the Ports section, like
Minecraft:

    cd ~/RetroPie/roms/ports
    vim Wolfenstein-3D.sh

Copy and paste the following lines:

    #!/bin/bash
    cd /home/pi/wolf4sdl
    /opt/retropie/supplementary/runcommand/runcommand.sh 0 "./wolf3d --res 640 480"

Remember to set the executable flag on the script before restarting the
Emulation Station GUI:

    chmod +x Wolfenstein-3D.sh

Done.

Restart and enjoy the game on your Picade like I do :-)

<blockquote class="twitter-tweet" data-lang="en">
<p lang="en" dir="ltr">

Wolfenstein 3D on the <a href="https://twitter.com/hashtag/Picade?src=hash">#Picade</a> 😍
<a href="https://t.co/8RUY4gidSv">pic.twitter.com/8RUY4gidSv</a>

</p>&mdash; Troglobit (@troglobit) <a href="https://twitter.com/troglobit/status/820614016498331648">January 15, 2017</a>

</blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>


[1]: https://en.wikipedia.org/wiki/Wolfenstein_3D
[2]: https://retropie.org.uk/
[3]: https://shop.pimoroni.com/products/picade

<!--
  -- Local Variables:
  -- mode: markdown
  -- End:
  -->
