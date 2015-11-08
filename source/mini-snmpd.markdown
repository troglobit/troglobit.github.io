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
very low impact on system resources.

The main features are:

- Supports SNMP version 1 and 2c
- Supports SNMP `get`, `getnext` and `getbulk`
- Supports both IPv4 and IPv6
- Supports communication over UDP and TCP sockets
- Supports the most important performance data (uptime, CPU load, memory usage)
- Supports the most important network data (bytes/packets in/out, error counts)
- Supports the most important disk data (disk space/inodes available/used/free)
- Tested with [net-snmp][1] and [cacti][2]


Example
-------

First start the daemon:

	$ sudo ./mini_snmpd -i eth0,eth1,wlan0

Then do an SNMP walk:

	$ snmpwalk -v2c -c public 127.0.0.1 .
	SNMPv2-MIB::sysDescr.0 = STRING: 
	SNMPv2-MIB::sysObjectID.0 = OID: SNMPv2-SMI::enterprises
	SNMPv2-MIB::sysUpTime.0 = Timeticks: (5954) 0:00:59.54
	SNMPv2-MIB::sysContact.0 = STRING: 
	SNMPv2-MIB::sysName.0 = STRING: luthien
	SNMPv2-MIB::sysLocation.0 = STRING: 
	IF-MIB::ifNumber.0 = INTEGER: 3
	IF-MIB::ifIndex.1 = INTEGER: 1
	IF-MIB::ifIndex.2 = INTEGER: 2
	IF-MIB::ifIndex.3 = INTEGER: 3
	IF-MIB::ifDescr.1 = STRING: eth0
	IF-MIB::ifDescr.2 = STRING: eth1
	IF-MIB::ifDescr.3 = STRING: wlan0
	IF-MIB::ifOperStatus.1 = INTEGER: up(1)
	IF-MIB::ifOperStatus.2 = INTEGER: lowerLayerDown(7)
	IF-MIB::ifOperStatus.3 = INTEGER: up(1)
	IF-MIB::ifInOctets.1 = Counter32: 1791842
	IF-MIB::ifInOctets.2 = Counter32: 0
	IF-MIB::ifInOctets.3 = Counter32: 1909293573
	IF-MIB::ifInUcastPkts.1 = Counter32: 18071
	IF-MIB::ifInUcastPkts.2 = Counter32: 0
	IF-MIB::ifInUcastPkts.3 = Counter32: 2010670
	IF-MIB::ifInDiscards.1 = Counter32: 12
	IF-MIB::ifInDiscards.2 = Counter32: 0
	IF-MIB::ifInDiscards.3 = Counter32: 0
	IF-MIB::ifInErrors.1 = Counter32: 0
	IF-MIB::ifInErrors.2 = Counter32: 0
	IF-MIB::ifInErrors.3 = Counter32: 0
	IF-MIB::ifOutOctets.1 = Counter32: 1199703
	IF-MIB::ifOutOctets.2 = Counter32: 0
	IF-MIB::ifOutOctets.3 = Counter32: 179994204
	IF-MIB::ifOutUcastPkts.1 = Counter32: 6437
	IF-MIB::ifOutUcastPkts.2 = Counter32: 0
	IF-MIB::ifOutUcastPkts.3 = Counter32: 1269885
	IF-MIB::ifOutDiscards.1 = Counter32: 0
	IF-MIB::ifOutDiscards.2 = Counter32: 0
	IF-MIB::ifOutDiscards.3 = Counter32: 0
	IF-MIB::ifOutErrors.1 = Counter32: 0
	IF-MIB::ifOutErrors.2 = Counter32: 0
	IF-MIB::ifOutErrors.3 = Counter32: 0
	HOST-RESOURCES-MIB::hrSystemUptime.0 = Timeticks: (4290528121) 496 days, 14:08:01.21
	UCD-SNMP-MIB::memTotalReal.0 = INTEGER: 8055412 kB
	UCD-SNMP-MIB::memAvailReal.0 = INTEGER: 961136 kB
	UCD-SNMP-MIB::memShared.0 = INTEGER: 0 kB
	UCD-SNMP-MIB::memBuffer.0 = INTEGER: 474372 kB
	UCD-SNMP-MIB::memCached.0 = INTEGER: 2473876 kB
	UCD-SNMP-MIB::dskIndex.1 = INTEGER: 1
	UCD-SNMP-MIB::dskPath.1 = STRING: /
	UCD-SNMP-MIB::dskTotal.1 = INTEGER: 222230176
	UCD-SNMP-MIB::dskAvail.1 = INTEGER: 20289308
	UCD-SNMP-MIB::dskUsed.1 = INTEGER: 201940864
	UCD-SNMP-MIB::dskPercent.1 = INTEGER: 91
	UCD-SNMP-MIB::dskPercentNode.1 = INTEGER: 12
	UCD-SNMP-MIB::laIndex.1 = INTEGER: 1
	UCD-SNMP-MIB::laIndex.2 = INTEGER: 2
	UCD-SNMP-MIB::laIndex.3 = INTEGER: 3
	UCD-SNMP-MIB::laNames.1 = STRING: Load-1
	UCD-SNMP-MIB::laNames.2 = STRING: Load-5
	UCD-SNMP-MIB::laNames.3 = STRING: Load-15
	UCD-SNMP-MIB::laLoad.1 = STRING: 0.06
	UCD-SNMP-MIB::laLoad.2 = STRING: 0.14
	UCD-SNMP-MIB::laLoad.3 = STRING: 0.28
	UCD-SNMP-MIB::laConfig.1 = STRING: 1
	UCD-SNMP-MIB::laConfig.2 = STRING: 5
	UCD-SNMP-MIB::laConfig.3 = STRING: 15
	UCD-SNMP-MIB::laLoadInt.1 = INTEGER: 6
	UCD-SNMP-MIB::laLoadInt.2 = INTEGER: 14
	UCD-SNMP-MIB::laLoadInt.3 = INTEGER: 28
	UCD-SNMP-MIB::ssCpuRawUser.0 = Counter32: 2263123
	UCD-SNMP-MIB::ssCpuRawNice.0 = Counter32: 8820
	UCD-SNMP-MIB::ssCpuRawSystem.0 = Counter32: 353194
	UCD-SNMP-MIB::ssCpuRawIdle.0 = Counter32: 46493489
	UCD-SNMP-MIB::ssRawInterrupts.0 = Counter32: 50186444
	UCD-SNMP-MIB::ssRawContexts.0 = Counter32: 243678129
	UCD-SNMP-MIB::ssRawContexts.0 = No more variables left in this MIB View (It is past the end of the MIB tree)


Project Info
------------

This version of mini-snmpd is the continuation of the hard work by
Robert Ernst.  Unfortunately his [mini-snmpd homepage][3] has gone
offline, that and the lack of updates over the last couple of years is
what prompted my setting up a [GitHub project][repo] to act as a focal
point for future development.  I've also taken the liberty of setting up
an [FTP mirror][ftp] of the previous releases I could find.

mini-snmpd is licensed under the [GNU GPL v2][LICENSE].

   * [Repository][repo]
   * [Issue Tracker](http://github.com/troglobit/mini-snmpd/issues)
   * [FTP][ftp]
   * [TODO][]
   * Man page [mini-snmpd.8](http://ftp.troglobit.com/mini-snmpd/mini-snmpd.html)

See also the old [Free(code) page](http://freecode.com/projects/minisnmpd).

[1]: http://net-snmp.org
[2]: http://net-snmp.net
[3]: https://web.archive.org/web/20150522170054/http://members.aon.at/linuxfreak/linux/mini_snmpd.html
[ftp]: ftp://ftp.troglobit/pub/mini-snmpd/
[repo]: http://github.com/troglobit/mini-snmpd
[TODO]: https://github.com/troglobit/mini-snmpd/blob/master/TODO
[LICENSE]: https://github.com/troglobit/mini-snmpd/blob/master/COPYING
