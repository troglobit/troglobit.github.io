---
name: mrouted
title: "The original multicast routing daemon"
date: 2017-01-01 20:28:00 +02:00
aliases: /mrouted.html
---
<img src="/images/dvmrp.png" style="width: 300px; height: 229px; float: right">

mrouted is an implementation of the IPv4 multicast routing protocol
DVMRP, [RFC 1075][].  It is capable of turning a UNIX workstation, or an
embedded Linux device, into a multicast router with tunnelling support,
in order to cross non-multicast-aware routers.

DVMRP is a distance vector based protocol, derived from RIP, suitable
for closely located multicast users in smaller networks.  It uses the
"flood and prune" method, where multicast is flooded until neighboring
routers opt out from unwanted multicast groups.  For a more thorough
explanation of the protocol, see [RFC 1075][].  In larger networks, with
sparsely located nodes, you might want to look into [pimd](/pimd.html).
For many use-cases static multicast routing, [smcroute](/smcroute.html)
may be sufficient.

The mrouted routing daemon was developed by David Waitzman, Craig
Partridge, Steve Deering, Ajit Thyagarajan, Bill Fenner, David Thaler
and Daniel Zappala.  With contributions by many others.

The last release by Mr. Fenner was 3.9-beta3 on April 26 1999 and
mrouted has been in "beta" status since then. Several prominent UNIX
operating systems, such as AIX, Solaris, [HP-UX][], BSD/OS, NetBSD,
FreeBSD, OpenBSD as well as most GNU/Linux based distributions have used
that beta as a de facto stable release, with (mostly) minor patches for
system adaptations. Over time however many dropped support, but Debian
and OpenBSD kept it under their wings.

In March 2003 the OpenBSD project, led by the fearless Theo de Raadt,
[managed to convince Stanford][1] to finally release mrouted under a
[fully free license][2], [the 3-clause BSD license][3].  In February
2005 [Debian nevertheless dropped mrouted][4] as an "obsolete protocol".

For a long time the OpenBSD team remained the sole guardian of this
project.

In 2010 this effort of bringing mrouted back to life was started. The
3.9.x stable series represent the first releases in over a
decade. Patches from all over the Internet, including OpenBSD, have
been integrated. Support for Debian/Ubuntu is included in the tree.

Issue tracker and GIT repository available at GitHub:

   * [Repository](http://github.com/troglobit/mrouted)
   * [ChangeLog](https://github.com/troglobit/mrouted/releases/tag/3.9.8)
   * [Issue Tracker](http://github.com/troglobit/mrouted/issues)
   * [mrouted-3.9.8.tar.bz2](ftp://ftp.troglobit.com/mrouted/mrouted-3.9.8.tar.bz2),
     [MD5](ftp://ftp.troglobit.com/mrouted/mrouted-3.9.8.tar.bz2.md5)

See also the [OpenHub page](https://www.openhub.net/p/mrouted/), or the
(sadly) dormant [Free(code) page](http://freecode.com/projects/mrouted).

Problems?  See the [multicast howto](/multicast-howto.html)

[1]: http://www.openbsd.org/cgi-bin/cvsweb/src/usr.sbin/mrouted/LICENSE)
[2]: https://github.com/troglobit/mrouted/blob/master/LICENSE
[3]: http://en.wikipedia.org/wiki/BSD_licenses
[4]: http://bugs.debian.org/cgi-bin/bugreport.cgi?bug=288112
[HP-UX]: http://docs.hp.com/en/B2355-90777/ch01s01.html
[RFC 1075]: http://tools.ietf.org/html/rfc1075
