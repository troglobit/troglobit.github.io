---
categories:
- networking
- ubuntu
- debian
comments: true
date: 2006-06-06T21:10:24Z
title: Laptop Network Configuration in Debian/Ubuntu
url: /2006/06/06/laptop-network-configuration-in-debian-slash-ubuntu/
aliases: /blog/2006/06/06/laptop-network-configuration-in-debian-slash-ubuntu/
---

I have three network cards in my laptop.  Two built-in and one PCMCIA
card.  I'm currently investigating how I can migrate from using the slow
and tedious gnome-network & C:o tools to the more integrated
Debian/Ubuntu tools `ifupdown`, `guessnet`, `ifplugd` and `waproamd`
instead.  As usual Debian has a fine and clever way of handling all this
with their `/etc/network/interface` file.  To learn more about this
[fine management interface][1] I'm reading the well written book
[The Debian System][2] by [Martin F. Krafft][3].  I highly recommend it
to others wanting to learn the essentials of their Debian (based)
system.

What I wanted to do is quite simple actually: the built-in wired
interface, named `eth0`, should on link-up default to a specific IP
address if ARP reveals a certain MAC on the network.  If plugged in to
an unknown net `eth0` should go up with DHCP.  If `eth0` is not up then
the built-in wlan card, `eth1`, should be tried with two different
ESSID's, work and home, in that order.  If none of the ESSID's could be
found any open WiFi access point should be the tried with DHCP.  The
last interface, `eth2`, is the PCMCIA card and is only used for the lab
net and always have the same IP address if plugged in completely
ignoring link status.

When I've implemented the above I'll post the result here.


[1]: http://www.debian.org/doc/manuals/reference/ch-gateway.en.html
[2]: http://debiansystem.info/
[3]: http://madduck.net/blog/

