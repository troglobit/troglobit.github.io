---
layout: page
name: mrouted
title: "The original multicast routing daemon"
sharing: true
footer: true
date: 2014-12-28 16:25
comments: false
---

<a href="https://github.com/troglobit/mrouted"><img style="position: absolute; top: 0; right: 0; border: none; box-shadow: none;" src="https://camo.githubusercontent.com/365986a132ccd6a44c23a9169022c0b5c890c387/68747470733a2f2f73332e616d617a6f6e6177732e636f6d2f6769746875622f726962626f6e732f666f726b6d655f72696768745f7265645f6161303030302e706e67" alt="Fork me on GitHub" data-canonical-src="https://s3.amazonaws.com/github/ribbons/forkme_right_red_aa0000.png"></a>

<img class="right" src="/images/dvmrp.png" style="width: 300px; height: 229px;">

mrouted is an implementation of the IPv4 DVMRP multicast routing
protocol, [RFC 1075](http://tools.ietf.org/html/rfc1075).  It is
capable of turning a UNIX workstation, or an embedded Linux device,
into a multicast router with tunnelling support, in order to cross
non-multicast-aware routers.

DVMRP is a distance vector based protocol, derived from RIP, suitable
for closely located multicast users in smaller networks.  It simply
floods all multicast streams to all routers, i.e. implicit join.  This
is also known as "flood and prune" since you can opt out from groups
you do not want.  For a detailed explanation of the protocol, see
[RFC 1075](http://tools.ietf.org/html/rfc1075).  For larger networks,
with sparsely located nodes, you might want to look into
[pimd](/pimd.html), and for static multicast routing
[smcroute](/smcroute.html) may be used.

The mrouted routing daemon was developed by David Waitzman, Craig
Partridge, Steve Deering, Ajit Thyagarajan, Bill Fenner, David Thaler
and Daniel Zappala.  With contributions by many others.

The last release by Mr. Fenner was 3.9-beta3 on April 26 1999 and
mrouted has been in "beta" status since then. Several prominent UNIX
operating systems, such as AIX, Solaris,
[HP-UX](http://docs.hp.com/en/B2355-90777/ch01s01.html), BSD/OS,
NetBSD, FreeBSD, OpenBSD as well as most GNU/Linux based distributions
have used that beta as a de facto stable release, with (mostly) minor
patches for system adaptations. Over time however many dropped
support, but Debian and OpenBSD kept it under their wings.

In March 2003 OpenBSD, led by the fearless Theo de Raadt,
[managed to convince Stanford](http://www.openbsd.org/cgi-bin/cvsweb/src/usr.sbin/mrouted/LICENSE)
to release mrouted under a
[fully free license](https://github.com/troglobit/mrouted/blob/master/LICENSE),
[the 3-clause BSD license](http://en.wikipedia.org/wiki/BSD_licenses).
In February 2005
[Debian nevertheless dropped mrouted](http://bugs.debian.org/cgi-bin/bugreport.cgi?bug=288112)
as an "obsolete protocol".

For a long time the OpenBSD team remained the sole guardian of this
project.

In 2010 this effort of bringing mrouted back to life was started. The
3.9.x stable series represent the first releases in over a
decade. Patches from all over the Internet, including OpenBSD, have
been integrated. Support for Debian/Ubuntu is included in the tree.

Issue tracker and GIT repository available at GitHub:

   * [Repository](http://github.com/troglobit/mrouted)
   * [ChangeLog](https://github.com/troglobit/mrouted/releases/tag/3.9.7)
   * [Issue Tracker](http://github.com/troglobit/mrouted/issues)
   * [mrouted-3.9.7.tar.bz2](ftp://ftp.troglobit.com/mrouted/mrouted-3.9.7.tar.bz2),
     [MD5](ftp://ftp.troglobit.com/mrouted/mrouted-3.9.7.tar.bz2.md5)

See also the [OpenHub page](https://www.openhub.net/p/mrouted/), or the
(sadly) dormant [Free(code) page](http://freecode.com/projects/mrouted).

Problems?  See the [multicast howto](/multicast-howto.html)

