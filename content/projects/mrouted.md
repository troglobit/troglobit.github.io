---
name: mrouted
title: The original multicast routing daemon
date: 2021-11-03 12:39:12 +01:00
aliases: /mrouted.html
---
![DVRMP](/images/dvmrp.png#floatright)

mrouted is an implementation of the IPv4 multicast routing protocol
DVMRP, [RFC 1075][].  It is capable of turning a UNIX workstation, or
Linux device, into a multicast router with tunneling support.

> Tunneling may be required to cross non-multicast-aware routers.

The Distantance Vector Multicast Routing Protocol (DVMRP), derived from
RIP, is suitable for smaller/dense networks.  It employs the "flood and
prune" method, where multicast is flooded until neighboring routers opt
out from unwanted multicast groups.  For a more thorough explanation of
the protocol, see [RFC 1075][].  In larger networks, with sparsely
located nodes, you might want to look into [pimd](/pimd.html).  For many
use-cases static multicast routing, [smcroute](/smcroute.html) may be
sufficient.

mrouted was developed by David Waitzman, Craig Partridge, Steve Deering,
Ajit Thyagarajan, Bill Fenner, David Thaler and Daniel Zappala.  With
contributions by many others.

The last release by Mr. Fenner was 3.9-beta3 on April 26 1999.  Several
prominent UNIX operating systems, such as AIX, Solaris, [HP-UX][],
BSD/OS, NetBSD, FreeBSD, OpenBSD as well as most GNU/Linux based
distributions have used that release, with (mostly) minor patches for
system adaptations.  However, over time many dropped support, but Debian
and OpenBSD kept it under their wings.

In March 2003 the OpenBSD project, led by the fearless Theo de Raadt,
[managed to convince Stanford][1] to finally release mrouted under a
[fully free license][2], [the 3-clause BSD license][3].  In February
2005 [Debian nevertheless dropped mrouted][4] as an "obsolete protocol".

For a long time the OpenBSD team remained the sole guardian of this
project.

In 2010 this effort of bringing mrouted back to life was started. The
3.9.x stable series represent the first releases in over a decade.
Patches from all over the Internet, including OpenBSD, have been
integrated.

In 2020, v4.0 and v4.1 were finally released after another 10 years of
hard work.  In 2021, v4.2, v4.3, and v4.4 were released.  The most
important news include:

   * A new command line tool, `mroutectl` to control and query status
   * Support for automatically detecting `altnet`, secondary addresses
   * Support for IGMPv3 and many config options to control IGMP
   * Static multicast routes, `phyint foo0 static-group GROUP`
   * Support for multiple multicast routing tables, `-t ID`

----

Issue tracker and GIT repository available at GitHub:

   * [Repository](https://github.com/troglobit/mrouted)
   * [ChangeLog](https://github.com/troglobit/mrouted/releases/tag/4.4)
   * [Issue Tracker](https://github.com/troglobit/mrouted/issues)
   * [mrouted-4.4.tar.gz](https://ftp.troglobit.com/mrouted/mrouted-4.4.tar.gz),
     [MD5](https://ftp.troglobit.com/mrouted/mrouted-4.4.tar.gz.md5),
     [SHA256](https://ftp.troglobit.com/mrouted/mrouted-4.4.tar.gz.sha256),

Manual pages availale here:

   * [mrouted(8)](https://man.troglobit.com/man8/mrouted.8.html)
   * [mroutectl(8)](https://man.troglobit.com/man8/mroutectl.8.html)
   * [mrouted.conf(5)](https://man.troglobit.com/man5/mrouted.conf.5.html)

See also the [OpenHub page](https://www.openhub.net/p/mrouted/), or the
(sadly) dormant [Free(code) page](http://freecode.com/projects/mrouted).

Problems?  See the [multicast howto](/multicast-howto.html)

[1]: https://www.openbsd.org/cgi-bin/cvsweb/src/usr.sbin/mrouted/LICENSE
[2]: https://github.com/troglobit/mrouted/blob/master/LICENSE
[3]: https://en.wikipedia.org/wiki/BSD_licenses
[4]: https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=288112
[HP-UX]: https://docs.hp.com/en/B2355-90777/ch01s01.html
[RFC 1075]: https://tools.ietf.org/html/rfc1075
