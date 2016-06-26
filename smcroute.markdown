---
layout: page
title: "SMCRoute &mdash; Static Multicast Routing Daemon"
sharing: true
footer: true
date: 2016-02-17 22:42 +01:00
comments: false
---

<a href="https://github.com/troglobit/smcroute"><img style="position: absolute; top: 0; right: 0; border: none; box-shadow: none;" src="https://camo.githubusercontent.com/365986a132ccd6a44c23a9169022c0b5c890c387/68747470733a2f2f73332e616d617a6f6e6177732e636f6d2f6769746875622f726962626f6e732f666f726b6d655f72696768745f7265645f6161303030302e706e67" alt="Fork me on GitHub" data-canonical-src="https://s3.amazonaws.com/github/ribbons/forkme_right_red_aa0000.png"></a>

SMCRoute is a daemon and command line tool to manipulate the multicast
routing table in the UNIX kernel.  Both FreeBSD and Linux kernels are
supported, but it may work on other systems as well.  SMCRoute can be
used as an alternative to dynamic multicast routing daemons like
[mrouted](/mrouted.html) or [pimd](/pimd.html) in situations where
(only) static multicast routes should be maintained and/or no proper
IGMP signaling exists.

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
    #       multicast to your router, or direct select groups they have
    #       such capabilities.  Usually MAC multicast filters exist.
    #
    #       The UNIX kernel usually limits the number of multicast groups
    #       a socket/client can joint.  In Linux 20 mgroup lines can be
    #       configured by default, but this can be configured using the
    #       /proc/sys/net/ipv4/igmp_max_memberships file.
    #
    # Similarily supported is setting mroutes. Removing mroutes is not
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

A very common question is why smcroute must be a daemon?  Why not just
a simple tool, like ip route, for unicast routes?  The answer lies in
how multicast is implemented in the UNIX kernel.  To be able to setup
multicast routes a program must connect to the multicast routing
socket in the kernel, when that socket is closed, which is done
automatically when a UNIX program ends, the kernel cleans up all
routes.  So you have to chose *one* of the multicast routing daemons
for all your multicast routing needs.

Problems?  See the [multicast howto](/multicast-howto.html)

SMCRoute was originally written by
[Carsten Schill](http://www.cschill.de/smcroute/).  Later on Julien
Blache, Todd Hayton and Micha Lenk picked up development for
[Debian](http://alioth.debian.org/projects/smcroute/).  Since 2011
Joachim Nilsson heads development at GitHub.  New features include
config file support, reloading config on SIGHUP, source-less on-demand
(*,G) routing, TTL scoping and support for disabling ALL interfaces
except the few used for multicast routing.

Issue tracker and GIT repository available at GitHub:

   * [Repository](http://github.com/troglobit/smcroute)
   * [smcroute-2.1.0.tar.xz](ftp://troglobit.com/smcroute/smcroute-2.1.0.tar.xz),
     [MD5](ftp://troglobit.com/smcroute/smcroute-2.1.0.tar.xz.md5)
     [GPG Sign](ftp://troglobit.com/smcroute/smcroute-2.1.0.tar.xz.asc)
   * [Issue Tracker](http://github.com/troglobit/smcroute/issues)
   * [Debian packages](http://packages.debian.org/smcroute)
   * [Ubuntu packages](http://packages.ubuntu.com/smcroute)
   * [Google Group](https://groups.google.com/forum/?fromgroups#!forum/smcroute)

See also the [OpenHub page](https://www.openhub.net/p/smcroute/), the
[Freshcode page](http://freshcode.club/projects/smcroute), or the now
dormant [Free(code) page](http://freecode.com/projects/smcroute).
