---
layout: page
title: "Mini SNMPd"
sharing: true
footer: true
date: 2015-11-07 02:12
comments: false
---

<a href="https://github.com/troglobit/mini-snmpd"><img style="position: absolute; top: 0; right: 0; border: none; box-shadow: none;" src="https://camo.githubusercontent.com/365986a132ccd6a44c23a9169022c0b5c890c387/68747470733a2f2f73332e616d617a6f6e6177732e636f6d2f6769746875622f726962626f6e732f666f726b6d655f72696768745f7265645f6161303030302e706e67" alt="Fork me on GitHub" data-canonical-src="https://s3.amazonaws.com/github/ribbons/forkme_right_red_aa0000.png"></a>

This is an SNMP server for embedded UNIX systems.  It is written in C
and easily portable to other UNIX systems, currently Linux and FreeBSD
kernels are supported.  The daemon is very small (~40 kiB) and does not
have nowhere near the feature set of [Net-SNMP][1], but has therefore a
very impact on system resources.

The main features of `mini-snmpd` are:

- Supports SNMP version 1 and 2c
- Supports SNMP `get`, `getnext` and `getbulk`
- Supports both IPv4 and IPv6
- Supports communication over UDP and TCP sockets
- Supports the most important performance data (uptime, CPU load, memory usage)
- Supports the most important network data (bytes/packets in/out, error counts)
- Supports the most important disk data (disk space/inodes available/used/free)
- Tested with [net-snmp][1] and [cacti][2]

This version of mini-snmpd is the continuation of the hard work by
Robert Ernst.  Unfortunately his [original homepage for mini-snmpd][3]
has gone offline, which is what initially prompted my setting up my
GitHub project to act as a focal point for future development.  The
software is licensed under the [GNU GPL v2][LICENSE].

Issue tracker and GIT repository available at GitHub:

   * [Repository](http://github.com/troglobit/mini-snmpd)
   * [Issue Tracker](http://github.com/troglobit/mini-snmpd/issues)
   * [FTP](ftp://ftp.troglobit/pub/mini-snmpd/)
   * [TODO][]

See also the old [Free(code) page](http://freecode.com/projects/minisnmpd).

[1]: http://net-snmp.org
[2]: http://net-snmp.net
[3]: https://web.archive.org/web/20150522170054/http://members.aon.at/linuxfreak/linux/mini_snmpd.html
[TODO]: https://github.com/troglobit/mini-snmpd/blob/master/TODO
[LICENSE]: https://github.com/troglobit/mini-snmpd/blob/master/COPYING
