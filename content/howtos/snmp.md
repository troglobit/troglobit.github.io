---
name:  "Playing with SNMP"
title: "HowTo play with SNMP"
date: 2023-09-04T20:24:00Z
orig-date: 2018-05-05 13:57:00 +02:00
aliases: [/howto-play-with-snmp.html]
categories: [ "snmp", "debian", "ubuntu", "howto" ]
---

This mini HowTo describes how to use the SNMP client tools to retrieve
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

The following commands installs snmpset/get/walk, base MIBs and all the
standard MIBs needed:

    sudo apt-get install snmp libsnmp-base snmp-mibs-downloader

On Debian/Ubuntu you have to also edit the file `/etc/snmp/snmp.conf` to
enable automatic loading of the downloaded MIBs, which is disabled by
default.  *Comment out* the line that reads:

    mibs :

Assuming you have already started `mini_snmpd`, say using port 16161 so
it doesn't have to run as root, you can now talk to it using:

    snmpwalk -v2c -c public 127.0.0.1:16161
    snmpget  -v2c -c public 127.0.0.1:16161 system.sysUpTime.0

On earlier versions of Ubuntu (pre 18.04) I've had trouble getting
net-snmp to use the UCD-SNMP-MIB, so I've added it explicitly to my
`~/snmp/snmp.conf`:

    mibs +UCD-SNMP-MIB

This MIB is in `/usr/share/snmp/mibs/` on my system ...


Installing Private MIB files
----------------------------

If you want to access a router/switch which uses private MIBs you can do
the same as I did for the UCD-SNMP-MIB:

    mkdir -p ~/.snmp/mibs
    cp COMPANY-SOME-MIB COMPANY-OID-MIB ~/.snmp/mibs/
    cat << EOF > ~/.snmp/snmp.conf
    # mibdirs /usr/share/snmp/mibs:/usr/share/mibs/ietf:/usr/share/mibs/iana:/usr/share/mibs/irtf:/usr/share/mibs/tubs
    mibdirs $HOME/.snmp/mibs
    mibs +COMPANY-SOME-MIB
    mibs +COMPANY-OID-MIB
    EOF

Usually, however, it's enough these days to just drop the MIB file(s) in
your `~/.snmp/mibs`.

Now you should be able to do this:

    snmpset -v2c -c private 192.168.2.200 COMPANY-SOME-MIB::reboot.0 i 1

which is quite an improvement over:

    snmpset -v2c -c private 192.168.2.200 1.3.6.1.4.1.16177.2.1.1.1.0 i 1


Useful Tests
------------

Walk all IP addresses

    snmpwalk  -v2c -c public 192.168.2.200 IP-MIB::ipAddrTable

Get loadavg

    snmpwalk  -v2c -c public 127.0.0.1:16161 UCD-SNMP-MIB::laLoad

Read interface table

    snmptable -v2c -c public 192.168.2.200 IF-MIB::ifTable

For more tips on using `snmpwalk`, see the excellent
[Net-SNMP wiki](http://www.net-snmp.org/wiki/index.php/TUT:snmpwalk)

[bug]: http://bugs.debian.org/cgi-bin/bugreport.cgi?bug=561578
