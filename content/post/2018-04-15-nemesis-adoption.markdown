---
title: "N E M E S I S"
date: 2018-04-15T16:11:23+02:00
subtitle: "The Resurrection"
tags: []
---

Summer is  almost upon us.  The  weather in Sweden has  been amazing the
last couple of weeks, sunny and  warm.  This has of course not prevented
me from pursuing my favorite indoor activity; coding!

This time around I've spent some  time to resurrect the packet injection
tool [Nemesis][].  It  was seemingly abandonded by  its original authors
almost 15 years  ago.  There was talk about porting  to libdnet, as well
as visions of a proper stand-alone  CLI instead of the command line tool
with options, but  it seems none of these plans  materialized.  The last
release, which was  v1.4, does not build against  libnet1, only libnet0.

However, I discovered  through the mailiing list there was  a CVS branch
integrating the porting work of some Cisco engineers to libnet1.

https://sourceforge.net/p/nemesis/mailman/message/177129/

I needed something simple for myself to generate IGMP packets, and since
I'm fluent in C, I decided to give Nemesis a whirl.  I finished the port
in a few days and began fixing some obvious bugs and incorporating fixes
from the mailing list and Debian.

The result is the upcoming v1.5 release, which is a long time coming ...

[Nemesis]: /projects/nemesis/
