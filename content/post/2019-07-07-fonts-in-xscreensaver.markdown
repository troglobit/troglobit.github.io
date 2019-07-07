---
title: "XScreenSaver Fonts"
subtitle: "in Debian/Ubuntu"
date: 2019-07-07 14:27:58 +0200
tags: []
---

I run Awesome on Ubuntu 19.04 with XScreenSaver.  This is a brief
writeup of what I did to fix the ugly default fonts.

<!-- more -->

Let's start with my `~/.Xdefaults` file.  The resources set are
used by XScreenSaver only if `~/.xscreensaver` is missing.  So
start by moving that out of the way:

```sh
mv ~/.xscreensaver ~/dot.xscreensaver
```

Here's the `~/.Xdefaults` file:

```
!font settings
xscreensaver.Dialog.headingFont:        -*-verdana-bold-r-*-*-28-*-*-*-*-*-*-*
xscreensaver.Dialog.bodyFont:           -*-verdana-medium-r-*-*-20-*-*-*-*-*-*-*
xscreensaver.Dialog.labelFont:          -*-verdana-medium-r-*-*-20-*-*-*-*-*-*-*
xscreensaver.Dialog.unameFont:          -*-verdana-medium-r-*-*-26-*-*-*-*-*-*-*
xscreensaver.Dialog.buttonFont:         -*-verdana-bold-r-*-*-20-*-*-*-*-*-*-*
xscreensaver.Dialog.dateFont:           -*-terminus-medium-r-*-*-16-*-*-*-*-*-*-*
xscreensaver.passwd.passwdFont:         -*-verdana-bold-r-*-*-20-*-*-*-*-*-*-*
```

To make TrueType fonts visible to old X applications one has to jump
through a few hoops.

```sh
cd /usr/share/fonts/truetype/msttcorefonts
sudo mkfontscale
sudo mkfontdir
xset +fp /usr/share/fonts/truetype/msttcorefonts/
xset fp rehash
xrdb -merge ~/.Xdefaults
xscreensaver-command -restart
```

