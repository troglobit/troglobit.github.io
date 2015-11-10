---
layout: post
title: "Awesome: Changing Next/Prev Tune in Spotify"
date: 2015-11-10 12:14:09 +0100
comments: true
categories: 
---

Back to using the [Awesome WM](http://awesome.naquadah.org/) in Ubuntu.
This time I'm setting up everything from scratch and first up is fixing
keybindings to control my main music player: Spotify!

Edit your `~/.config/awesome/rc.lua` with Emacs (obviously).  If you do
not have an rc file, simply copy the system `/etc/xdb/awesome/rc.lua`:

    globalkeys = awful.util.table.join(globalkeys,
    	       awful.key({}, "XF86AudioRaiseVolume", function () awful.util.spawn("amixer -D pulse sset Master 5%+", false) end),
	       awful.key({}, "XF86AudioLowerVolume", function () awful.util.spawn("amixer -D pulse sset Master 5%-", false) end),
	       awful.key({}, "XF86AudioMute", function() awful.util.spawn('amixer -D pulse sset Master 1+ toggle') end),
	       awful.key({ }, "XF86AudioNext", function () awful.util.spawn("dbus-send --print-reply --dest=org.mpris.MediaPlayer2.spotify /org/mpris/MediaPlayer2 org.mpris.MediaPlayer2.Player.Next")end),
	       awful.key({ }, "XF86AudioPrev", function () awful.util.spawn("dbus-send --print-reply --dest=org.mpris.MediaPlayer2.spotify /org/mpris/MediaPlayer2 org.mpris.MediaPlayer2.Player.Previous")end),
	       awful.key({ }, "XF86AudioPlay", function () awful.util.spawn("dbus-send --print-reply --dest=org.mpris.MediaPlayer2.spotify /org/mpris/MediaPlayer2 org.mpris.MediaPlayer2.Player.PlayPause")end),
	       awful.key({ }, "XF86AudioStop", function () awful.util.spawn("dbus-send --print-reply --dest=org.mpris.MediaPlayer2.spotify /org/mpris/MediaPlayer2 org.mpris.MediaPlayer2.Player.Stop")end))
    root.keys(globalkeys)

That's it.

