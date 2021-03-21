---
categories:
- reminder
- misc
comments: true
date: 2009-02-14T21:13:41Z
title: Reminder to Self
url: /2009/02/14/reminder-to-self/
aliases: /blog/2009/02/14/reminder-to-self/
---

How to recode AVI-files to be able to write them to a [Video CD][1].

    ffmpeg -i original.avi -target pal-vcd copy.mpg
    vcdimager -t vcd1 -l "Title" -c vcd.cue -b vcd.bin copy.mpg
    cdrdao write --device /dev/cdrw vcd.cue

**Note:** `vcdimager` is able to take multiple .mpg files as argument,
very useful when burning multiple family videos to the same disc!

* * *

Tip from [Johan Risberg][2] on how to be able to reattach a UTF-8 Linux
screen session from Cygwin.  First, start irssi in a screen from a
Linux login with `LANG=LANG en_US.utf8`.  Then reattach to the screen,
preferably using `-xRR`, with `LANG=C` from [Cygwin][3].

[1]: http://en.wikipedia.org/wiki/Video_CD
[2]: http://nummerfem.se/
[3]: http://www.cygwin.org/cygwin

