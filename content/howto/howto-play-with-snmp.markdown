---
name:  "Playing with SNMP"
title: "HowTo play with SNMP"
date: 2015-11-07 12:49:00 +02:00
categories: [ "snmp", "debian", "ubuntu", "howto" ]
---

This mini HowTo is about how you can use SNMP client tools to retrieve
*human readable* information from devices running an SNMP daemon.  In
the examples below [mini-snmpd](/mini-snmpd.html) is used as the daemon
and as client both the command line [net-snmp](http://www.net-snmp.org)
tool and the [snmpB](http://sourceforge.net/projects/snmpb/) GUI are
used.

Personally I use both Ubuntu and Debian, so the tools I use to download
the clients will reflect that.  See your respective UNIX distro's help
pages for how to install these client tools in your operating system.


Installing SNMP in Ubuntu
-------------------------

The following command installs snmpset/get/walk, base MIBs and all
standard MIBs needed.

    sudo apt-get install snmp libsnmp-base snmp-mibs-downloader

Assuming you have already started `mini_snmpd` on your client, you
should be able to do the following:

    snmpwalk -v2c -c public 127.0.0.1
	snmpget -c public -v 2c 127.0.0.1 system.sysUpTime.0

If you experience problems, see Debian [bug #561578][bug] for details on
how to edit and fix your system default `/etc/snmp/snmp.conf`

On my system I've had trouble getting net-snmp to use the UCD-SNMP-MIB,
so I've added it explicitly to my `~/snmp/snmp.conf`:

	mibs +UCD-SNMP-MIB

This MIB is in `/usr/share/snmp/mibs/` on my system ...


Installing Private MIB files
----------------------------

If you want to access a router/switch which uses private MIBs you can do
the same as I did for the UCD-SNMP-MIB:

    mkdir -p ~/.snmp/mibs
	cp ALL-PRIVATE-MIBS ~/.snmp/mibs/`basename $file .my`
	cat << EOF > ~/.snmp/snmp.conf
	mibdirs /usr/share/snmp/mibs:/usr/share/mibs/ietf:/usr/share/mibs/iana:/usr/share/mibs/irtf:/usr/share/mibs/tubs:$HOME/.snmp/mibs
	mibs +COMPANY-SOME-MIB
	mibs +COMPANY-OID-MIB
	EOF

Now you should be able to do this:

	snmpset -c private -v 2c 192.168.2.200 COMPANY-SOME-MIB::reboot.0 i 1

which is quite an improvement over:

	snmpset -c private -v 2c 192.168.2.200 1.3.6.1.4.1.16177.2.1.1.1.0 i 1


Useful Tests
------------

Walk all IP addresses

	snmpwalk -v2c -c public 192.168.2.20 1.3.6.1.2.1.4.20 

Get loadavg

	snmpwalk -v2c -c public 127.0.0.1 UCD-SNMP-MIB::laLoad

For more tips on using `snmpwalk`, see the
[Net-SNMP wiki](http://www.net-snmp.org/wiki/index.php/TUT:snmpwalk)

[bug]: http://bugs.debian.org/cgi-bin/bugreport.cgi?bug=561578
