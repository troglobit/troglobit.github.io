---
layout: post
title: "Ubuntu 8.10rc1 &mdash; NetworkManager WTF?!"
date: 2008-10-27 19:50:03 +0200
comments: true
categories: 
---

I tried upgrading from Hardy Heron to Intrepid Ibex this weekend.  Big
mistake.  If I disregard the total dpkg meltdown that I, as usually, had
to resolve manually there still remains this recurring madness called
Network Manager.

I'm an engineer.  I have a masters degree in computer engineering.  I
have worked professionally with GNU/Linux since 2000 and been a die hard
user of it since I left OS/2 behind in 1996.  But come on!  Why does it
take a software engineer to perform a simple upgrade?  Even worse, why
do I have to be a network engineering specialist to figure out why
simple wired ethernet doesn't work out of the box?

Network Manager fails completely to:

1. Select wired network over an open Telia hotspot
2. Distinguish between my primary and secondary wired interfaces
3. Completely loses my `/etc/resolv.conf` all the time 

Sorry, but that is completely inexcusable!  [Network Manager][1], you
suck!  "Pain-Free Networking", my ass! :-P

[1]: http://www.gnome.org/projects/NetworkManager/
