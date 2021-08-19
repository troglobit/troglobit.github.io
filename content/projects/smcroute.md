---
name: SMCRoute
title: "Static Multicast Routing Daemon"
date: 2021-08-19 15:46:00 +02:00
aliases: /smcroute.html
---

SMCRoute is a daemon and command line tool to manipulate the multicast
routing table in the UNIX kernel.  Both FreeBSD and Linux kernels are
supported, but it may work on other systems as well.

SMCRoute can be used as an alternative to dynamic multicast routing
daemons like [mrouted](/mrouted.html) or [pimd](/pimd.html) when (only)
static multicast routes should be maintained or no proper signalling
exists.

The full documentation of SMCRoute is available in the manual pages, see
[smcrouted(8)][], [smcroutectl(8)][], and [smcroute.conf(5)][].

> Problems? See the [Multicast HowTo](/multicast-howto.html) for help!


Features
--------

- Full IPv4 and IPv6 support
- Configuration file support, [/etc/smcroute.conf](smcroute-conf.html)
- Configuration include support, `/etc/smcroute.d/*.conf`
- Support for restarting and reloading the `.conf` on `SIGHUP`
- Support for seamless reload and update on `SIGHUP`, i.e., established
  multicast flows are not disturbed by reloading the configuration or
  adding/removing outbound interfaces to existing routes
- Source-less on-demand routing, a.k.a. (*,G) based static routing
- Support for ranges, (S/LEN,G) and/or (S,G/LEN), or (S/LEN,G/LEN),
  be careful though since this expands to multiple kernel routes
- Source specific group join support
- Optional built-in [mrdisc][] support, [RFC4286][] (IPv4 only)
- Support for multiple routing tables on Linux
- Client with built-in support to show routes and joined groups
- Interface wildcard matching, `eth+` matches `eth0, eth15`


Why a Daemon?
-------------

One common question is why SMCRoute must be a daemon, why not just a
simple tool, like `ip route` for unicast routes?  The answer is that to
be able to add multicast routes a program must connect to the multicast
routing socket in the kernel, when that socket is closed, which is done
automatically when a UNIX program ends, the kernel cleans up all routes.


Origin & References
-------------------

SMCRoute was originally written by [Carsten Schill][].  Later on Julien
Blache, Todd Hayton and Micha Lenk picked up development for [Debian][].

Since 2011 Joachim Nilsson heads development at [GitHub][].  New
features include config file support, reloading config on `SIGHUP`,
source-less on-demand (*,G) routing, TTL scoping and support for
disabling ALL interfaces except the few used for multicast routing.

Issue tracker and GIT repository available at [GitHub][].

* [Repository][GitHub]
* [smcroute-2.5.0.tar.xz](https://ftp.troglobit.com/smcroute/smcroute-2.5.0.tar.gz),
  [MD5](https://ftp.troglobit.com/smcroute/smcroute-2.5.0.tar.gz.md5)
* [Issue Tracker](https://github.com/troglobit/smcroute/issues)
* [Debian packages](https://packages.debian.org/smcroute)
* [Ubuntu packages](https://packages.ubuntu.com/smcroute)

See also the [OpenHub page](https://www.openhub.net/p/smcroute/), the
[Freshcode page](http://freshcode.club/projects/smcroute), or the now
dormant [Free(code) page](http://freecode.com/projects/smcroute).

[smcrouted(8)]:    https://man.troglobit.com/man8/smcrouted.8.html
[smcroutectl(8)]:  https://man.troglobit.com/man8/smcroutectl.8.html
[smcroute.conf(5)]:https://man.troglobit.com/man5/smcroute.conf.5.html
[mrdisc]:          https://github.com/troglobit/mrdisc
[RFC4286]:         https://tools.ietf.org/html/rfc4286
[GitHub]:          https://github.com/troglobit/smcroute
[Debian]:          https://alioth.debian.org/projects/smcroute/
[Carsten Schill]:  http://www.cschill.de/smcroute/
