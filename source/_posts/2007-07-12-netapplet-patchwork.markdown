---
layout: post
title: "Netapplet Patchwork"
date: 2007-07-12 11:37:38 +0200
comments: true
categories: 
- networking
- ubuntu
- debian
---

I think I've finally done it &mdash; achieved my ultimate goal of
combining the awesome powers of the Debian `/etc/network/interfaces`
file with `guessnet`, `wpa_supplicant`, `ifplugd` and now also with
netapplet!

I've been looking for a way to just point-and-click to select a
different access point, both at home and at work.  To that end I have a
pretty advanced interfaces file that automatically detects where I am,
what I am connected with (cable/wireless) and then, like magic, sets it
all up.

Several times now I have tried, really hard, to get used to and live my
life with [Network Manager][1], but it's just plain impossible.  It
still doesn't integrate well with Debian ifupdown et consortes and until
now I've been using `ifup` to select different wireless mappings:

```
$ sudo ifdown eth2; sudo ifup eth2=WLAN
Password: ***********
[Use crappy work AP to access VPN tunnel]
$ sudo ifdown eth2; sudo ifup eth2=NETLET
Password: ***********
[Use family WRT54GL access point]
```

For the last couple of days I've been looking into getting the old
(obsoleted?) Netapplet from Novell.  Since I'm a Ubuntu user I'm using
the Debian patched-up version by [Matthew Garrett][mjg59] (thanks!).  I
remember using it a couple of years back, at that time I think it worked
but now it didn't &mdash; not well at least.  I found several bugs and
quirks that I have now fixed and published in my own "repository".

I have published everything as a Bazaar branch, one changeset per patch
(small patches), at [http://vmlinux.org/jocke/bzr/netapplet-1.0.8][2],
which can be viewed [here][3] using bzrweb.

[1]: http://www.gnome.org/projects/NetworkManager/
[2]: https://web.archive.org/web/20090528101921/http://vmlinux.org/jocke/bzr/netapplet-1.0.8/
[3]: https://web.archive.org/web/20090528101921/http://vmlinux.org/jocke/bzr/
[mjg59]: http://mjg59.livejournal.com/
