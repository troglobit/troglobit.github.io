---
layout: page
title: pimd | The original PIM-SM daemon
sharing: true
footer: true
date: 2014-12-28 13:37
comments: false
---

<a href="https://github.com/troglobit/pimd"><img style="position: absolute; top: 0; right: 0; border: none; box-shadow: none;" src="https://camo.githubusercontent.com/365986a132ccd6a44c23a9169022c0b5c890c387/68747470733a2f2f73332e616d617a6f6e6177732e636f6d2f6769746875622f726962626f6e732f666f726b6d655f72696768745f7265645f6161303030302e706e67" alt="Fork me on GitHub" data-canonical-src="https://s3.amazonaws.com/github/ribbons/forkme_right_red_aa0000.png"></a>

{% img right /images/pim-sm.gif 222 155 %}

pimd is a lightweight stand-alone PIM-SM v2 multicast routing
daemon. It is the original USC (netweb/catarina.usc.edu)
implementation of the protocol, according to
[RFC 2362](http://tools.ietf.org/html/rfc2362).

Thanks to OpenBSD freeing [mrouted](/mrouted.shtml), pimd is now also
fully free to use under the 3-clause
[BSD license](https://github.com/troglobit/pimd/blob/master/LICENSE).

Protocol Independent Multicast, PIM, allows existing networks to route
IP multicast, regardless of what unicast routing protocol is in use.
It is designed to use the existing routing tables to make its
multicast routing decisions.  PIM-SM is suitable for sparsely located
multicast subscribers, for dense mode operation
[mrouted](/mrouted.html) is recommended, and for static multicast
routing [smcroute](/smcroute.html) may be used.

pimd was written by Ahmed Helmy, George Edmond "Rusty" Eddy, and
Pavlin Ivanov Radoslavov. With contributions by many others.

Issue tracker and GIT repository available at GitHub:

   * [Repository](http://github.com/troglobit/pimd)
   * [ChangeLog](https://github.com/troglobit/pimd/releases/tag/2.2.0)
   * [pimd-2.2.0.tar.bz2](ftp://troglobit.com/pimd/pimd-2.2.0.tar.bz2),
     [MD5](ftp://troglobit.com/pimd/pimd-2.2.0.tar.bz2.md5)
   * [Issue Tracker](http://github.com/troglobit/pimd/issues)
   * [Debian packages](http://packages.debian.org/pimd)
   * [Ubuntu packages](http://packages.ubuntu.com/pimd)

See also the [OpenHub page](https://www.openhub.net/p/pimd/), or the
(sadly) dormant [Free(code) page](http://freecode.com/projects/pimd),

Problems?  See the [multicast howto](/multicast-howto.html)


References
----------

The PIM-SM protocol was first defined in
[RFC 2362](http://tools.ietf.org/html/rfc2362) and later updated in
[RFC 4601](http://tools.ietf.org/html/rfc4601), with additions in
[RFC 5059](http://tools.ietf.org/html/rfc5059) and
[RFC 5796](http://tools.ietf.org/html/rfc5796).

   * The PIM-SM GateD implementation from ISI. (defunct)
   * The PIM-DM GateD implementation from the University of Oregon. (defunct)
   * The [pimd-dense](http://antc.uoregon.edu/PIMDM/pimd-dense.html)
     University of Oregon standalone implementation, based on the USC
     pimd.
   * The PIM-SM implementation from [the XORP project](http://www.xorp.org/)
   * The PIM IPv6
     [pim6sd](http://clarinet.u-strasbg.fr/~hoerdt/dev/pim6sd_linux/)
     by Mickael Hoerdt at LSIIT Laboratory, based on the USC pimd.
   * [MRD6](http://fivebits.net/proj/mrd6/), an IPv6 Multicast Router
   * The upcoming [Quagga](http://www.quagga.net/) PIM-SSM,
     [qpimpd](https://savannah.nongnu.org/projects/qpimd)

Mailing Lists
-------------

The following mailing list is directly related to PIM:

   * pim@ietf.org: the IETF PIM Working Group mailing list.
   * To subscribe/unsubscribe,
     [https://www.ietf.org/mailman/listinfo/pim/](https://www.ietf.org/mailman/listinfo/pim/)
   * Archives available at
     [http://www.ietf.org/mail-archive/web/pim/current/maillist.html](http://www.ietf.org/mail-archive/web/pim/current/maillist.html)


