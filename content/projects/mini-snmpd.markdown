---
name: mini-snmpd
title: "Mini SNMP daemon"
date: 2020-02-23 09:00:00 +02:00
aliases: /mini-snmpd.html
---

This is an SNMP server for small and embedded systems, currently Linux
and FreeBSD are supported.  It is easily portable to other UNIX systems
since it's written in C.  The daemon is very small (~40 kiB) and does
not have nowhere near the feature set of [Net-SNMP][1], therefore it has
a very low impact on system resources.

See my mini HowTo: [Playing with SNMP](/howto-play-with-snmp.html) for a
quick introduction to setting up the SNMP tools and MIBs to avoid having
to use numerical OIDs.

**Features:**

- Supports SNMP version 1 and 2c
- Supports SNMP `get`, `getnext` and `getbulk`
- Supports both IPv4 and IPv6
- Supports communication over UDP and TCP sockets
- Supports the most important performance data (uptime, CPU load, memory usage)
- Supports the most important network data (bytes/packets in/out, error counts)
- Supports the most important disk data (disk space/inodes available/used/free)
- Tested with [net-snmp][1], [cacti][2], and [MRTG][3]

<a id="example"></a>

Example
-------

First start the daemon:

	$ sudo ./mini-snmpd -i eth0,eth1,wlan0

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

<a id="small"></a>

Building Really Small Binaries
------------------------------

By simply calling `./configure && make` you don't really get a small
mini-snmpd binary.  Sure, most people know about setting `CFLAGS=-Os`
before calling the configure script -- that's how you reach the ~40 kiB
mentioned above.

	CFLAGS="-Os" ./configure && make clean all && strip mini-snmpd && ll mini-snmpd && size mini-snmpd
	-rwxrwxr-x 1 jocke jocke 39520 nov  8 21:35 mini-snmpd*
	   text	   data	    bss	    dec	    hex	 filename
	  32766	   1028	  16032	  49826	   c2a2	 mini-snmpd

To get really crazy with things you can try this, it works for me but
YMMV as usual:

	CFLAGS="-W -Wall -Os -U_FORTIFY_SOURCE -fno-stack-protector -fomit-frame-pointer -ffunction-sections -fdata-sections -Wl,--gc-sections -fno-asynchronous-unwind-tables -fmerge-all-constants -fno-ident -Wl,-z,norelro -Wl,--hash-style=gnu -Wl,--build-id=none " ./configure --disable-ipv6
	make clean all && strip -S --strip-unneeded --remove-section=.note.gnu.gold-version --remove-section=.comment --remove-section=.note --remove-section=.note.gnu.build-id --remove-section=.note.ABI-tag mini-snmpd && ll mini-snmpd && size mini-snmpd
    -rwxrwxr-x 1 jocke jocke 30696 nov  8 21:37 mini-snmpd*
       text	   data	    bss	    dec	    hex	 filename
      27305	    964	  15968	  44237	   accd	 mini-snmpd

This insane amount of arguments to GCC saves you ~9kiB.  Which begs the
question, is there anything else you can do, how low can we go?!  Let me
introduce you to [upx](http://upx.sourceforge.net/):

	upx --ultra-brute mini-snmpd && ll mini-snmpd && size mini-snmpd
                           Ultimate Packer for eXecutables
                              Copyright (C) 1996 - 2013
	UPX 3.91        Markus Oberhumer, Laszlo Molnar & John Reiser   Sep 30th 2013
	
            File size         Ratio      Format      Name
       --------------------   ------   -----------   -----------
         30696 ->     15604   50.83%  linux/ElfAMD   mini-snmpd
    
    Packed 1 file.
    -rwxrwxr-x 1 jocke jocke 15604 nov  8 21:37 mini-snmpd*
       text	   data	    bss	    dec	    hex	 filename
          0	      0	      0	      0	      0	 mini-snmpd

Yeah, running `size` on the binary afterwards is kinda useless, but
*WOW*!  Using upx actually cut the size down to almost half of what we
got with the insane arguments above -- saved ~15kiB on a 30kiB binary!

On embedded targets some of these tricks may just about save you if
you're running out of flash.  However, there are often nasty compiler
bugs to be found just by changing optimization level to `-Os`.  So when
it comes to embedded I always recommend playing it safe and going with
`-O2` and no further optimizations, unless you want to spend a lot of
time looking for weird bugs!

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
   * [Issue Tracker](http://github.com/troglobit/mini-snmpd/issues)
   * [mini-snmpd-1.6.tar.gz](ftp://ftp.troglobit.com/mini-snmpd/mini-snmpd-1.6.tar.gz),
     [MD5](ftp://ftp.troglobit.com/mini-snmpd/mini-snmpd-1.6.tar.gz.md5),
   * [ChangeLog](https://github.com/troglobit/mini-snmpd/releases/tag/v1.6)
   * [TODO][]
   * Man page [mini-snmpd.8](http://ftp.troglobit.com/mini-snmpd/mini-snmpd.html) (outdated)

See also the old [Free(code) page](http://freecode.com/projects/minisnmpd).

[1]: http://net-snmp.org
[2]: http://net-snmp.net
[3]: http://oss.oetiker.ch/mrtg/
[4]: https://web.archive.org/web/20150522170054/http://members.aon.at/linuxfreak/linux/mini_snmpd.html
[ftp]: ftp://ftp.troglobit.com/mini-snmpd/
[repo]: http://github.com/troglobit/mini-snmpd
[TODO]: https://github.com/troglobit/mini-snmpd/blob/master/TODO
[LICENSE]: https://github.com/troglobit/mini-snmpd/blob/master/COPYING
