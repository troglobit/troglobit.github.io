---
layout: page
name: pimd
title: The original PIM-SM daemon
sharing: true
footer: true
date: 2015-11-15 19:26 +01:00
comments: false
---

<a href="https://github.com/troglobit/pimd"><img style="position: absolute; top: 0; right: 0; border: none; box-shadow: none;" src="https://camo.githubusercontent.com/365986a132ccd6a44c23a9169022c0b5c890c387/68747470733a2f2f73332e616d617a6f6e6177732e636f6d2f6769746875622f726962626f6e732f666f726b6d655f72696768745f7265645f6161303030302e706e67" alt="Fork me on GitHub" data-canonical-src="https://s3.amazonaws.com/github/ribbons/forkme_right_red_aa0000.png" /></a>

<img class="right" src="/images/pim-sm.gif" style="width: 222px; height: 155px;" />

pimd is a lightweight standalone PIM-SM/SSM v2 multicast routing daemon.
It is the original USC (netweb/catarina.usc.edu) implementation of the
protocol, [RFC 2362][1].  Today pimd strives for [RFC 4601][2]
compliance, with the v2.3.0 release supporting both PIM-SSM and IGMPv3.

In 2003 the OpenBSD project [managed to convince Stanford][stanford] in
to change the license of [mrouted](/mrouted.shtml).  This in turn also
freed pimd, since it is built with DNA strands from mrouted.  pimd is
fully free to use under the simplified 3-clause [BSD license][license].

------

Protocol Independent Multicast, PIM, allows existing networks to route
IP multicast, regardless of what unicast routing protocol is in use.  It
is designed to use existing routing tables to make its multicast routing
decisions.  PIM-SM is suitable for sparsely located multicast
subscribers, for dense mode operation [mrouted](/mrouted.html) is
recommended, and for static multicast routing [smcroute](/smcroute.html)
may be used.

pimd was written by Ahmed Helmy, George Edmond "Rusty" Eddy, and Pavlin
Ivanov Radoslavov.  With contributions by many others, lately the most
notable contributions have been by Markus Veranen and Mika Joutsenvirta.

------

Issue tracker and GIT repository available at GitHub:

   * [Repository](http://github.com/troglobit/pimd)
   * [ChangeLog](https://github.com/troglobit/pimd/releases/tag/2.3.2)
   * [pimd-2.3.2.tar.gz](ftp://troglobit.com/pimd/pimd-2.3.2.tar.gz),
     [MD5](ftp://troglobit.com/pimd/pimd-2.3.2.tar.gz.md5)
   * [Issue Tracker](http://github.com/troglobit/pimd/issues)
   * [Debian packages](http://packages.debian.org/pimd)
   * [Ubuntu packages](http://packages.ubuntu.com/pimd)

See also the [OpenHub page](https://www.openhub.net/p/pimd/), the
[Fresh(code) page](http://freshcode.club/projects/pimd), or the now
now dormant [Free(code) page](http://freecode.com/projects/pimd).

Problems?  See the [multicast howto](/multicast-howto.html)


References
----------

The PIM-SM protocol was first defined in [RFC 2362][1] and later updated
in [RFC 4601][2], with additions in [RFC 5059][3] and [RFC 5796][4].

   * The PIM-SM GateD implementation from ISI. (defunct)
   * The PIM-DM GateD implementation from the University of Oregon. (defunct)
   * The [pimd-dense][dense]
     University of Oregon standalone implementation, based on the USC
     pimd.
   * The PIM-SM implementation from [the XORP project](http://www.xorp.org/)
   * The PIM IPv6 [pim6sd][] by Mickael Hoerdt at LSIIT Laboratory,
     based on USC pimd.
   * [MRD6](http://fivebits.net/proj/mrd6/), an IPv6 Multicast Router
   * The upcoming [Quagga](http://www.quagga.net/) PIM-SSM,
     [qpimpd](https://savannah.nongnu.org/projects/qpimd) -- Now merged!


Mailing Lists
-------------

The following mailing list is directly related to PIM:

   * <mailto:pim@ietf.org>: the IETF PIM Working Group mailing list.
   * To subscribe/unsubscribe, <https://www.ietf.org/mailman/listinfo/pim/>
   * Archives available at <http://www.ietf.org/mail-archive/web/pim/current/maillist.html>

[1]: http://tools.ietf.org/html/rfc2362
[2]: http://tools.ietf.org/html/rfc4601
[3]: http://tools.ietf.org/html/rfc5059
[4]: http://tools.ietf.org/html/rfc5796
[dense]: http://antc.uoregon.edu/PIMDM/pimd-dense.html
[pim6sd]: http://clarinet.u-strasbg.fr/~hoerdt/dev/pim6sd_linux/
[stanford]: http://www.openbsd.org/cgi-bin/cvsweb/src/usr.sbin/mrouted/LICENSE
[license]: https://github.com/troglobit/pimd/blob/master/LICENSE
