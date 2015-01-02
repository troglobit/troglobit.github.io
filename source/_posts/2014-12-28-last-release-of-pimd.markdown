---
layout: post
title: "pimd v2.2.0 -- The last release of pimd?"
date: 2014-12-28 13:42:18 +0100
comments: true
categories: 
---

On one of the last days of 2014 I release [pimd](/pimd.html)
[v2.2.0](https://github.com/troglobit/pimd/releases/tag/2.2.0), which
is an awesome release with a lot of new features and bug fixes!

However, it could very well be the last release I do.  Even though its
one of my most popular projects on
[GitHub](https://github.com/troglobit) I have not had enough time to
dedicate to it over the years.  I will continue to do fixes and merge
pull requests until someone else steps up to take over.  There is also
the distinct possibility that the
[Xorp PIM-SM](https://github.com/greearb/xorp.ct) or the new
[Quagga PIM-SSM](https://github.com/udhos/qpimd) implementations will
(finally) make good old pimd completely redundant.

At [work](http://www.westermo.com) we will likely start using the
Quagga PIM rather than pimd in WeOS.

For now though, enjoy pimd v2.2.0.  It's been tested in both my Qemu
based virtual testbed and a few setups using Linux' `netns` feature in
[CORE](http://www.nrl.navy.mil/itd/ncs/products/core) -- awesome
little proggy! :-)

### Changes & New Features
- Add IP fragmentation support for PIM register messages,
  by Michael Fine of Cumulus Networks
- Support `/LEN` syntax in `phyint` to complement `masklen LEN`, issue #12
- Add support for /31 networks, point-to-point, thanks to Apollon Oikonomopoulos
- Remove old broken SNMP support
- OpenBSD inspired cleanup (deregister)
  http://cvsweb.openbsd.org/cgi-bin/cvsweb/src/usr.sbin/mrouted/
- General code cleanup, shorten local variable names, func decl. etc.
- Support for router alert IP option in IGMP queries
- Support for reading IGMPv3 membership reports
- Update IGMP code to support FreeBSD >= 8.x
- Retry read of routing tables on FreeBSD
- Fix join/leve of ALL PIM Routers for FreeBSD and other UNIX kernels
- Tested on FreeBSD, NetBSD and OpenBSD
- Add very simple homegrown configure script
- Update and document support for `rp_address`, `cand_rp`, and `cand_bootstrap_router`
- Add new `spt_threshold` to replace existing `switch_register_threshold`
  and `switch_data_threshold` settings.  Cisco like and easier to understand

### Bug Fixes
- Fix to avoid infinite loop during unicast send failure, by Alex Tessmer
- Fix bug in bootstrap when configured as candidate RP, issue #15
- Fix segfault in `accept_igmp()`, issue #29
- Fix default source preference, should be 101 (not 1024!)
- Fix `ip_len` handling on older BSD's, thanks to Olivier Cochard-Labbé, issue #23
- Fix default prefix len in static RP example in `pimd.conf`, should be /4
- Fix issue #31: Make IGMP query interval and querier timeout configurable
- Fix issue #33: pimd does not work in background under FreeBSD
- Fix issue #35: support for timing out other queriers from mrouted
- Hopefully fix issue #22: Crash in (S,G) state when neighbor is lost
- Misc. bug fixes thanks to Coverity Scan, static code analysis tool
  https://scan.coverity.com/projects/3319

