---
layout: post
title: "Multicast testing, made easy!"
date: 2016-03-07 01:06:02 +0100
comments: true
categories:
- multicast
- unix
---

Spent the better part of the weekend fixing up my old mcjoin tool.  Been
meaning to get around to it for ages, and now it seems I had finally
worked up a huge need for it.  So here it is, v2.0

<https://github.com/troglobit/mcjoin/releases/tag/v2.0>

I wanted a reliable, simple, and UNIX-y tool to just test things for me.
So I cleaned up the old mcjoin project, first by migrating it from the
toolbox repo, and then added send/receive capabilities.

Most of the time I simply want to see a resulting IGMP join in
Wireshark, and see it bite in a switch or routing daemon.  So join is
the default, and also the name of the tool.  My favourite testing group
is the default, 225.1.2.3, so you only need to start the tool and you're
off.  To send to the same default group (225.1.2.3), simply add `-s` to
the sender side.

If you ever need anything else, e.g. routing multicast, there's even a
man page.  It mentions setting the TTL and other such nastiness :)
