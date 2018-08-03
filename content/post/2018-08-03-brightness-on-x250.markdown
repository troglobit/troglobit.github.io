---
title: "Display Brightness in Awesome on ThinkPad X250"
subtitle: ""
date: 2018-08-03 16:15:00 +0200
tags: []
---

Having just received a new (used) ThinkPad X250 I felt somewhat lost
when I realized the display brightness keys didn't do anything in my 
[Awesome WM][] setup.

<!--more-->

My previous ThinkPads were a T430s and an X230 and it turns out Lenovo
had changed from HW controlled to SW controlled in this new generation.

A quick google later and I had found Fredrik Haikarainen's [light][]
and bound it to the approriate keys in my `.config/awesome/rc.lua`:

    -- Brightness
    globalkeys = gears.table.join(globalkeys,
    	awful.key({}, "XF86MonBrightnessUp",   function () awful.util.spawn("light -A 10") end),
    	awful.key({}, "XF86MonBrightnessDown", function () awful.util.spawn("light -U 10") end))
    root.keys(globalkeys)

Actually, I tried running light from the command line first, without
running `sudo make install`, and that didn't work so I was about to give
up.  Turns out `light` needs to be SUID root because of reasons.  Oh well,
that we should be able fix relatively easy.

Great little tool, much recommend!

[light]: https://github.com/haikarainen/light
[Awesome WM]: https://awesomewm.org
