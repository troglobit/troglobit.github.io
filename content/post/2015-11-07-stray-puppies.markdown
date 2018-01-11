---
categories:
- personal
- projects
comments: true
date: 2015-11-07T02:32:12Z
title: Stray Puppies
url: /2015/11/07/stray-puppies/
aliases: /blog/2015/11/07/stray-puppies/
---

Sometimes I just cannot help myself.  It's like finding a stray puppy,
or abandonded kitten ...

... I recently decided to adopt [mini-snmpd](/mini-snmpd.html) since the
[original upstream][1] site had passed into the great beyond.  At this
point in my life almost everyone I know can tell you I have no warm
fuzzy feels for SNMP, at all.  So why did I even consider this to begin
with?!

Well, I have to confess that there are certain things that SNMP can be
really useful for.  Most of it is remote monitoring, and since I work
with embedded systems a lot, SNMP can be quite a handy tool.

I first ran into mini-snmpd when I created [TroglOS][2], by forking
[miniroot][3], and what struck me immediately was how small and simple
it was.  The code was also in reasonably good shape, so it had passed my
initial quality control.  Suddenly one day I could no longer download it
when building TroglOS, so I had to do something!

My plans for mini-snmpd are quite humble.  I'm currently cleaning it up
a bit, adding configure script for all optional features, testing
portability to FreeBSD and [integrating (good) patches][4] from various
sources scattered around the web.

I will not add lots of new features, but as always I'm your humble patch
monkey, so if you [submit a pull reqeust][5] at GitHub it'll probably be
merged and put into a release.

Cheers

[1]: http://members.aon.at/linuxfreak/linux/mini_snmpd.html
[2]: https://github.com/troglobit/troglos
[3]: https://github.com/hno/miniroot
[4]: https://github.com/javiplx/mini-snmpd
[5]: https://github.com/troglobit/mini-snmpd
