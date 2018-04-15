---
name: Nemesis
title: "N E M E S I S"
date: 2018-04-15 10:33:00 +02:00
aliases: /nemesis.html
---

Nemesis is a command-line network packet crafting and injection utility
for UNIX-like and Windows systems.  Well suited for testing Network
Intrusion Detection Systems, firewalls, IP stacks and a variety of other
tasks.  As a command-line driven utility, it is perfect for automation
and scripting.

Nemesis can natively craft and inject ARP, DNS, ETHERNET, ICMP, IGMP,
IP, OSPF, RIP, TCP and UDP packets.  Using the IP and the Ethernet
injection modes, almost any custom packet can be crafted and injected.

Issue tracker and GIT repository available at GitHub:

   * [Repository](http://github.com/troglobit/nemesis)
   * [Issue Tracker](http://github.com/troglobit/nemesis/issues)
   * [ChangeLog](https://github.com/troglobit/nemesis/blob/master/ChangeLog.md)


Examples
--------

Send TCP packet (SYN/ACK) with payload from file 'foo' to target's ssh
port from 192.168.1.1 to 192.168.2.2. (`-v` allows a `stdout` visual of
current injected packet):

    nemesis tcp -v -S 192.168.1.1 -D 192.168.2.2 -fSA -y 22 -P foo

Send UDP packet from 10.11.12.13:11111 to 10.1.1.2's name-service port
with a payload read from a file 'bindpkt'. (again `-v` is used in order to
see confirmation of our injected packet):

    nemesis udp -v -S 10.11.12.13 -D 10.1.1.2 -x 11111 -y 53 -P bindpkt

Send ICMP REDIRECT (network) packet from 10.10.10.3 to 10.10.10.1 with
preferred gateway as source address. Here we want no output to go to
`stdout` -- which would be ideal as a component in a batch job via a
shell script:

    nemesis icmp -S 10.10.10.3 -D 10.10.10.1 -G 10.10.10.3 -qR

Send ARP packet through device `ne0` (eg. my OpenBSD pcmcia nic) from
hardware source address 00:01:02:03:04:05 with IP source address
10.11.30.5 to destination IP address 10.10.15.1 with broadcast
destination hardware address. In other words, who-has the mac address of
10.10.15.1, tell 10.11.30.5 - assuming 00:01:02:03:04:05 is the source
mac address of our `ne0` device:

    nemesis arp -v -d ne0 -H 0:1:2:3:4:5 -S 10.11.30.5 -D 10.10.15.1


Documentation
-------------

   * [nemesis.1](http://nemesis.sourceforge.net/manpages/nemesis.1.html)
   * [nemesis-arp.1](http://nemesis.sourceforge.net/manpages/nemesis-arp.1.html)
   * [nemesis-dns.1](http://nemesis.sourceforge.net/manpages/nemesis-dns.1.html)
   * [nemesis-ethernet.1](http://nemesis.sourceforge.net/manpages/nemesis-ethernet.1.html)
   * [nemesis-icmp.1](http://nemesis.sourceforge.net/manpages/nemesis-icmp.1.html)
   * [nemesis-igmp.1](http://nemesis.sourceforge.net/manpages/nemesis-igmp.1.html)
   * [nemesis-ip.1](http://nemesis.sourceforge.net/manpages/nemesis-ip.1.html)
   * [nemesis-ospf.1](http://nemesis.sourceforge.net/manpages/nemesis-ospf.1.html)
   * [nemesis-rip.1](http://nemesis.sourceforge.net/manpages/nemesis-rip.1.html)
   * [nemesis-tcp.1](http://nemesis.sourceforge.net/manpages/nemesis-tcp.1.html)
   * [nemesis-udp.1](http://nemesis.sourceforge.net/manpages/nemesis-udp.1.html)


Origin & References
-------------------

Nemesis was created by Mark Grimes in 1999, in 2001 Jeff Nathan took
over maintainership.  In 2018, after more than a decade of inactivity,
Joachim Nilsson stepped in, converted from CVS to GIT and merged the
stale libnet-1.1 upgrade branch from 2005.

