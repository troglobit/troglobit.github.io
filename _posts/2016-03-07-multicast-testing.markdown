---
layout: post
title: "Multicast testing, made easy!"
date: 2016-03-07 01:06:02 +0100
comments: true
categories:
- multicast
- unix
---

<a href="https://github.com/troglobit/mcjoin"><img style="position: absolute; top: 0; right: 0; border: none; box-shadow: none;" src="https://camo.githubusercontent.com/365986a132ccd6a44c23a9169022c0b5c890c387/68747470733a2f2f73332e616d617a6f6e6177732e636f6d2f6769746875622f726962626f6e732f666f726b6d655f72696768745f7265645f6161303030302e706e67" alt="Fork me on GitHub" data-canonical-src="https://s3.amazonaws.com/github/ribbons/forkme_right_red_aa0000.png" /></a>

For the better part of the last ten years I have been working with
multicast in one way or another.  I've used many different tools for
testing, but on most systems I usually resort to `ping(1)` and
`tcpdump(1)`, which are quite sufficient.  However, you often need to
tell bridges (switches) to open up multicast in your general direction
for your pings to get through, so you need to send an IGMP "join" first.

Way back in 2006 I stumbled upon a neat tool called `mcjoin`, written by
David Stevens and announced in
[this posting to LKML](https://lkml.org/lkml/2004/8/5/143).  I started
improving and adding features to it over the years.

<!-- more -->

Now, my interest and fascination with multicast only grew with time,
and despite elegant tools like `mgen(1)` and `omping(1)` I was never
quite happy.

When releasing SMCRoute v2.1.0 recently, and currently working on the
pimd v2.3.2 release, I was so tired of having to do so many manual steps
just to verify correct operation of a routing daemon.  Therefore I spent
the better part of the past weekend fixing up my old mcjoin tool.

I wanted a reliable, simple, and UNIX-y tool to just test things for me.
So I cleaned up the old mcjoin project, first by migrating it from the
toolbox repo, removing confusing command line options, improving and
simplifying the syntax, and then I added send/receive capabilities.

I've been meaning to get around to this for ages, and now it seems I had
finally had enough.  So here it is, v2.0:

* **Project**: <https://github.com/troglobit/mcjoin/>
* **ChangeLog**: <https://github.com/troglobit/mcjoin/releases/tag/v2.0>

Most of the time I simply want to see a resulting IGMP join message in
Wireshark, watch a group entry pop up in a switch's FDB or a routing
daemon's forwarding table.  So, join is the default operation, and also
continues to be the name of the tool.  My favourite testing group is set
as the default, 225.1.2.3, so you only need to start the tool and you're
off.  To send to the same default group (225.1.2.3), simply add `-s` to
the sender side.

	sender$ mcjoin -s
	^C
	sender$

	receiver$ mcjoin
	joined group 225.1.2.3 on eth0 ...
	..................................................................^C
	Received total: 66 packets
	receiver$

If you ever need anything else, e.g. routing multicast, there's even a
man page.  It mentions setting the TTL and other such nastiness :)
