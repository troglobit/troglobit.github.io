---
name: smcroute.conf
date: 2017-06-13 23:15:00 +02:00
title: smcroute.conf example
url: /project/smcroute/smcroute-conf.html
aliases: /smcroute-conf.html
---
<!--more-->
```conf
#
# smcroute.conf example
#
# The configuration file supports joining multicast groups, to use
# Layer-2 signaling so that switches and routers open up multicast
# traffic to your interfaces.  Leave is not supported, remove the
# mgroup and SIGHUP your daemon, or send a specific leave command.
#
# NOTE: Use of the mgroup command should be avoided if possible.
#       Instead configure "router ports" or similar on the switches
#       or bridges on your LAN.  This to have them direct all the
#       multicast to your router, or select groups if they have
#       such capabilities.  Usually MAC multicast filters exist.
#
#       Some switches IGMP snooping implementations support mrdisc,
#       RFC4286, which SMCRoute use to advertise on source interfaces.
#
#       The UNIX kernel usually limits the number of multicast groups
#       a socket/client can join.  Linux for instance supports only
#       20 groups by default, but this can be configured using the
#       /proc/sys/net/ipv4/igmp_max_memberships file.
#
# Similarly supported is setting mroutes. Removing mroutes is not
# supported, remove/comment out the mroute or send a remove command.
#
# Syntax:
#   phyint IFNAME <disable|enable> [ttl-threshold <1-255>]
#   mgroup from IFNAME group MCGROUP
#   mroute from IFNAME [source ADDRESS] group MCGROUP to IFNAME [IFNAME ...]

# This example disables the creation of a multicast VIF for WiFi
# interface wlan0.  The kernel (at least Linux) sets the ALLMULTI
# flag for all interfaces that have a VIF enabled.  Hence, it can
# cause quite a bit of unnecessary traffic to reach the CPU if too
# many interfaces have a VIF (or MIF in IPv6 lingo).  Only enable
# interfaces required for inbound and outbound traffic.
# phyint wlan0 disable
phyint eth0 enable ttl-threshold 11
phyint eth1 enable ttl-threshold 3
phyint eth2 enable ttl-threshold 5
phyint virbr0 enable ttl-threshold 5

# The following example instructs the kernel to join the multicast
# group 225.1.2.3 on interface eth0.  Followed by setting up an
# mroute of the same multicast stream, but from the explicit sender
# 192.168.1.42 on the eth0 network and forward to eth1 and eth2.
#
mgroup from eth0 group 225.1.2.3
mroute from eth0 group 225.1.2.3 source 192.168.1.42 to eth1 eth2

mgroup from virbr0 group 225.1.2.4
mroute from virbr0 group 225.1.2.4 source 192.168.123.110 to eth0

# Here we allow routing of multicast to group 225.3.2.1 from ANY
# source coming in from interface eth0 and forward to eth1 and eth2.
# NOTE: Routing from ANY source is currently only available for IPv4
#       multicast.
mgroup from eth0 group 225.3.2.1
mroute from eth0 group 225.3.2.1 to eth1 eth2

# It is also possible to route a complete (*,G/LEN) range 
mroute from eth0 group 239.255.255.0/24 to eth1

# The following example instructs the kernel to join the source
# specific multicast group 225.2.1.3 on interface eth0 with
# source 192.168.10.25.
mgroup from eth0 group 225.2.1.3 source 192.168.10.25
```
