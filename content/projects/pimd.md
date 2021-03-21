---
name: pimd
title: The original PIM-SM daemon
date: 2019-04-28 08:50:00 +02:00
aliases: /pimd.html
---
<img src="/images/pim-sm.gif" style="width: 340px; height: 220px; float: right" />

Protocol Independent Multicast, PIM, allows existing networks to route
IP multicast, regardless of what unicast routing protocol is in use.  It
is designed to use existing routing tables to make its multicast routing
decisions.  PIM-SM is suitable for sparsely located multicast
subscribers, for dense mode operation [mrouted](/mrouted.html) is
recommended, and for static multicast routing [smcroute](/smcroute.html)
may be used.

pimd is a lightweight standalone PIM-SM/SSM v2 multicast routing daemon.
It is the original USC (netweb/catarina.usc.edu) implementation of the
protocol, [RFC 2362][1].  Today pimd strives for full RFC compliance,
including [RFC 4601][2], [RFC 5059][3], and [RFC 5796][4], with the
v2.3.0 release supporting both PIM-SSM and IGMPv3.

pimd was originally written by Ahmed Helmy, George Edmond "Rusty" Eddy,
and Pavlin Ivanov Radoslavov, with contributions by many others.  Lately
the most notable contributors have been Markus Veranen, Joonas Ruohonen,
and Mika Joutsenvirta.

In 2003 the OpenBSD project [managed to convince Stanford][stanford]
to change the license of [mrouted](/mrouted.shtml).  This in turn also
freed pimd, since it is built with DNA strands from mrouted.  pimd is
fully free to use under the simplified 3-clause [BSD license][license].

Issue tracker and GIT repository available at GitHub:

   * [Repository](http://github.com/troglobit/pimd)
   * [ChangeLog](https://github.com/troglobit/pimd/releases/tag/2.3.2)
   * [pimd-2.3.2.tar.gz](ftp://ftp.troglobit.com/pimd/pimd-2.3.2.tar.gz),
     [MD5](ftp://ftp.troglobit.com/pimd/pimd-2.3.2.tar.gz.md5)
   * [Issue Tracker](http://github.com/troglobit/pimd/issues)
   * [Debian packages](http://packages.debian.org/pimd)
   * [Ubuntu packages](http://packages.ubuntu.com/pimd)

See also the [OpenHub page](https://www.openhub.net/p/pimd/), the
[Fresh(code) page](http://freshcode.club/projects/pimd), or the now
now dormant [Free(code) page](http://freecode.com/projects/pimd).


Troubleshooting
---------------

See the [multicast howto](/multicast-howto.html)


Mailing Lists
-------------

The following mailing list is directly related to PIM:

   * <mailto:pim@ietf.org>: the IETF PIM Working Group mailing list.
   * To subscribe/unsubscribe, <https://www.ietf.org/mailman/listinfo/pim/>
   * Archives available at <http://www.ietf.org/mail-archive/web/pim/current/maillist.html>


References
----------

The PIM-SM protocol was first defined in [RFC 2362][1] and later updated
in [RFC 4601][2], with additions in [RFC 5059][3] and [RFC 5796][4].

   * The PIM-SM GateD implementation from ISI. (defunct)
   * The PIM-DM GateD implementation from the University of Oregon. (defunct)
   * The [pimd-dense][dense]
     University of Oregon standalone implementation, based on the USC
     pimd.  Now available from the [mcast-tools][] project
   * [MRD6](http://fivebits.net/proj/mrd6/), an IPv6 Multicast Router.
     Project has been deprecated, recommending pim6sd, but is available
     at <https://github.com/hugosantos/mrd6>
   * The PIM IPv6 [pim6sd][] by Mickael Hoerdt at LSIIT Laboratory,
     based on USC pimd.  Now available from the [mcast-tools][] project
   * [The XORP project][xorp] supports both IPv4 and IPv6 PIM-SM,
     currently maintained by [Ben Greear at GitHub][xorp.ct]
   * [Quagga][] and [FRR][] support PIM-SSM (IPv4 only?).  Originally
     developed by Everton da Silva Marques as [qpimd][]


[1]: http://tools.ietf.org/html/rfc2362
[2]: http://tools.ietf.org/html/rfc4601
[3]: http://tools.ietf.org/html/rfc5059
[4]: http://tools.ietf.org/html/rfc5796
[xorp]: http://www.xorp.org/
[xorp.ct]: https://github.com/greearb/xorp.ct
[FRR]: https://github.com/FRRouting/frr
[Quagga]: http://www.quagga.net/
[qpimd]: https://savannah.nongnu.org/projects/qpimd
[dense]: http://antc.uoregon.edu/PIMDM/pimd-dense.html
[pim6sd]: http://clarinet.u-strasbg.fr/~hoerdt/dev/pim6sd_linux/
[stanford]: http://www.openbsd.org/cgi-bin/cvsweb/src/usr.sbin/mrouted/LICENSE
[license]: https://github.com/troglobit/pimd/blob/master/LICENSE
[mcast-tools]: https://github.com/F0rth/mcast-tools
