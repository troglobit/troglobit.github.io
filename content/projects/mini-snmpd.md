---
name: mini-snmpd
title: "Mini SNMP daemon"
date: 2020-02-23 09:00:00 +02:00
lastmod: 2026-07-04 09:28:00 +0100
aliases: /mini-snmpd.html
---

This is an SNMP server for small and embedded systems, currently Linux
and FreeBSD are supported.  It is easily portable to other UNIX systems
since it's written in C.  A stripped binary is ~80 kiB and comes nowhere
near the feature set of [Net-SNMP][1], so it has a very low impact on
system resources — a good fit for routers, NAS boxes, and other embedded
devices.

See my mini HowTo: [Playing with SNMP](/howto-play-with-snmp.html) for a
quick introduction to setting up the SNMP tools and MIBs to avoid having
to use numerical OIDs.

**Features:**

- SNMP version 1 and 2c, with `get`, `getnext`, and `getbulk`
- Dual-stack: serves IPv4 and IPv6 at the same time
- Communication over both UDP and TCP (so SNMP over an SSH tunnel works)
- SNMPv2c notifications (traps): coldStart, linkUp/linkDown, and
  authentication failure
- Real-time interface tracking on Linux via netlink — interfaces created
  or removed after start-up appear and disappear without a restart
- Reloads its configuration on `SIGHUP`, even after dropping privileges
- Optional `/etc/mini-snmpd.conf` (libConfuse), combined with the command line
- Custom static responses on any OID, e.g. to emulate another device
- Read-only, and can drop root privileges with `-u`
- Wide MIB coverage: SNMPv2-MIB (system group, agent counters, sysORTable),
  IF-MIB (ifTable and the 64-bit ifXTable), IP-MIB (including the RFC 4293
  ipAddressTable, so IPv6 addresses are discoverable), TCP-MIB, UDP-MIB,
  HOST-RESOURCES-MIB (memory, storage, processors, system), UCD-SNMP-MIB
  (memory, disk, load average, CPU), and LM-SENSORS-MIB temperatures on Linux
- Tested with [net-snmp][1], [cacti][2], and [MRTG][3]

See [MIBS.md][mibs] for the complete list of objects served.

<a id="example"></a>

Example
-------

With no `-i` the daemon monitors every interface (loopback first); here we
pick a couple and run on a high port so it need not be root:

	$ ./mini-snmpd -n -p 16161 -D "Ubuntu 24.04 LTS" -C "ops@example.com" \
	               -L "Server room" -i eth0,eth1

An SNMP walk then returns the system, interface, and host data.  The
excerpt below is abbreviated — see [MIBS.md][mibs] for everything served:

	$ snmpwalk -v2c -c public 127.0.0.1:16161
	SNMPv2-MIB::sysDescr.0 = STRING: Ubuntu 24.04 LTS
	SNMPv2-MIB::sysObjectID.0 = OID: SNMPv2-SMI::enterprises
	DISMAN-EVENT-MIB::sysUpTimeInstance = Timeticks: (104) 0:00:01.04
	SNMPv2-MIB::sysContact.0 = STRING: ops@example.com
	SNMPv2-MIB::sysName.0 = STRING: server
	SNMPv2-MIB::sysLocation.0 = STRING: Server room
	SNMPv2-MIB::sysServices.0 = INTEGER: 79
	SNMPv2-MIB::sysORID.1 = OID: SNMPv2-MIB::snmpMIB
	...
	IF-MIB::ifDescr.1 = STRING: eth0
	IF-MIB::ifDescr.2 = STRING: eth1
	...
	HOST-RESOURCES-MIB::hrSystemDate.0 = STRING: 2026-7-4,10:12:33.0,+2:0
	HOST-RESOURCES-MIB::hrSystemProcesses.0 = Gauge32: 214
	HOST-RESOURCES-MIB::hrProcessorLoad.1 = INTEGER: 3
	IP-MIB::ipAddressIfIndex.ipv4."192.0.2.10" = INTEGER: 2
	IP-MIB::ipAddressIfIndex.ipv6."fe:80:00:00:00:00:00:00:...:01" = INTEGER: 2
	LM-SENSORS-MIB::lmTempSensorsValue.1 = Gauge32: 42000
	SNMPv2-MIB::snmpInPkts.0 = Counter32: 12

New in 2.0, mini-snmpd can also send traps.  Point it at a manager with
`-T addr[:port]` (repeatable):

	$ ./mini-snmpd -n -c public -i eth0 -T 192.0.2.5

It emits `coldStart` at start-up, `linkUp`/`linkDown` as interfaces change
state, and `authenticationFailure` on a wrong community.  With the optional
`.conf` file you can also emulate another device by pinning a fixed string
to an OID, e.g. to look like an HP JetDirect print server:

	custom ".1.3.6.1.4.1.11.2.3.9.1.1.7.0" {
	        value = "MFG:Hewlett-Packard;MDL:HP LaserJet 1020;CLS:PRINTER;"
	}

<a id="small"></a>

Building Really Small Binaries
------------------------------

A default `./configure && make` now bundles the `.conf` parser, ethtool
statistics, and the test suite, so the binary is ~100 kiB.  For the
smallest build, opt out of what you do not need and set `CFLAGS=-Os`:

	CFLAGS="-Os" ./configure --without-config --disable-ethtool --disable-ipv6
	make clean all && strip mini-snmpd && size mini-snmpd
	   text	   data	    bss	    dec	    hex	 filename
	  65347	   2824	 349504	 417675	  65f8b	 mini-snmpd

That is ~80 kiB on disk; most of the `bss` is the fixed-size MIB table,
demand-zeroed, so it does not weigh on resident memory until used.

You can push further.  The figures below are from an older, smaller
release, but the technique is unchanged — a pile of GCC and linker flags
strips off several more kiB:

	CFLAGS="-W -Wall -Os -U_FORTIFY_SOURCE -fno-stack-protector -fomit-frame-pointer -ffunction-sections -fdata-sections -Wl,--gc-sections -fno-asynchronous-unwind-tables -fmerge-all-constants -fno-ident -Wl,-z,norelro -Wl,--hash-style=gnu -Wl,--build-id=none " ./configure --without-config --disable-ipv6
	make clean all && strip -S --strip-unneeded --remove-section=.note.gnu.gold-version --remove-section=.comment --remove-section=.note --remove-section=.note.gnu.build-id --remove-section=.note.ABI-tag mini-snmpd

And [upx](http://upx.sourceforge.net/) can then roughly halve the on-disk
size again:

	upx --ultra-brute mini-snmpd

On embedded targets these tricks may just save you if you are running out
of flash.  There are, however, often nasty compiler bugs to be found just
by changing the optimization level to `-Os`.  So for embedded I always
recommend playing it safe with `-O2` and no further optimizations, unless
you want to spend a lot of time chasing weird bugs!

<a id="info"></a>

Project Info
------------

This is the continuation of the hard work on mini-snmpd by Robert Ernst.
Unfortunately his [mini-snmpd homepage][4] has gone offline, so that and
the lack of updates over the last couple of years is what prompted my
setting up a [GitHub project][repo] to act as a focal point for future
development.  I've also taken the liberty of setting up an
[FTP mirror][ftp] of any previous releases I could find.  If you happen
to come by any older version(s), send me an email! :)

mini-snmpd is licensed under the [GNU GPL v2][LICENSE].

   * [Repository][repo]
   * [Download release tarballs](https://github.com/troglobit/mini-snmpd/releases)
   * [Release notes / ChangeLog](https://github.com/troglobit/mini-snmpd/releases)
   * [Issue Tracker](http://github.com/troglobit/mini-snmpd/issues)
   * [Supported MIBs][mibs]
   * [Extending mini-snmpd](https://github.com/troglobit/mini-snmpd/blob/master/doc/extending.md)
   * [TODO](https://github.com/troglobit/mini-snmpd/blob/master/doc/TODO)
   * Man pages: [mini-snmpd.8](https://man.troglobit.com/man8/mini-snmpd.8.html),
     [mini-snmpd.conf.5](https://man.troglobit.com/man5/mini-snmpd.conf.5.html)

See also the old [Free(code) page](http://freecode.com/projects/minisnmpd).

[1]: https://www.net-snmp.org/
[2]: https://www.cacti.net/
[3]: https://oss.oetiker.ch/mrtg/
[4]: https://web.archive.org/web/20150522170054/http://members.aon.at/linuxfreak/linux/mini_snmpd.html
[ftp]: https://ftp.troglobit.com/mini-snmpd/
[repo]: https://github.com/troglobit/mini-snmpd
[mibs]: https://github.com/troglobit/mini-snmpd/blob/master/doc/MIBS.md
[LICENSE]: https://github.com/troglobit/mini-snmpd/blob/master/COPYING
