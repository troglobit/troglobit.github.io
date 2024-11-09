---
name: smcroute.conf
date: 2021-08-19 15:46:00 +02:00
title: smcroute.conf example
url: /projects/smcroute/smcroute-conf.html
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
#       Some switch manufacturers support mrdisc, RFC4286, which
#       SMCRoute can use to advertise itself on source interfaces.
#       If availble, use that instead of mgroup.
#
# Similarly supported is setting mroutes.  Removing mroutes is not
# supported, remove/comment out the mroute from the .conf file, or
# send a remove command with smcroutectl.
#
# Syntax:
#   phyint IFNAME <enable|disable> [mrdisc] [ttl-threshold <1-255>]
#   mgroup from IIF [source ADDR[/LEN]] group GROUP[/LEN]
#   mroute from IIF [source ADDR[/LEN]] group GROUP[/LEN] to OIF [OIF ...]
#   include /path/to/*.conf

# This example assumes smcrouted was started with the `-N` flag.
# Only enable interfaces required for inbound and outbound traffic.
phyint eth0 enable ttl-threshold 11
phyint eth1 enable ttl-threshold 3
phyint eth2 enable ttl-threshold 5
phyint virbr0 enable ttl-threshold 5

# Instruct the kernel to join the multicast group 225.1.2.3 on interface
# eth0.  Then add an mroute of the same multicast stream, from the host
# 192.168.1.42 on interface eth0 and forward to eth1 and eth2.
mgroup from eth0                     group 225.1.2.3
mroute from eth0 source 192.168.1.42 group 225.1.2.3 to eth1 eth2

# Similar example, but using source-specific group join
mgroup from virbr0 source 192.168.123.110 group 225.1.2.4
mroute from virbr0 source 192.168.123.110 group 225.1.2.4 to eth0

# Allow multicast for group 225.3.2.1, from ANY source, ingressing on
# interface eth0 to be forwarded to eth1 and eth2.  When the kernel
# receives a frame from unknown multicast sender, it asks smcrouted who
# use this "template" to match against, if the ingressing interface and
# group matches, smcrouted installs an (S,G) route in the kernel MFC.
mgroup from eth0 group 225.3.2.1
mroute from eth0 group 225.3.2.1 to eth1 eth2

# The previous is an example of the (*,G) support.  It is also possible
# to specify a range of such rules.
mgroup from eth0 group 225.0.0.0/24
mroute from eth0 group 225.0.0.0/24 to eth1 eth2

# Include any snippet in /etc/smcroute.d/, but please remember that
# all phyint statements must be read first.
include /etc/smcroute.d/*.conf
```
