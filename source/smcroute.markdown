---
layout: page
title: "SMCRoute &mdash; Static Multicast Routing Daemon"
sharing: true
footer: true
date: 2013-07-11 16:47
comments: false
---

SMCRoute is a daemon and command line tool to manipulate the multicast
routing table in a UNIX kernel.  It can be used as an alternative to
dynamic multicast routers like [mrouted](/mrouted.html) or
[pimd](/pimd.html) in situations where (only) static multicast routes
should be maintained and/or no proper IGMP signaling exists.

    #
    # smcroute.conf example
    #
    # The configuration file supports joining multicast groups, to use
    # Layer-2 signaling so that switches and routers open up multicast
    # traffic to your interfaces.  Leave is not supported, remove the
    # mgroup and SIGHUP your daemon, or send a specific leave command.
    #
    # Similarily supported is setting mroutes. Removing mroutes is not
    # supported, remove/comment out the mroute or send a remove command.
    #
    # Syntax:
    #   mgroup from IFNAME group MCGROUP
    #   mroute from IFNAME [source ADDRESS] group MCGROUP to IFNAME [IFNAME ...]
    #
    # The following example instructs the kernel to join the multicast
    # group 225.1.2.3 on interface eth0.  Followed by setting up an
    # mroute of the same multicast stream, but from the explicit sender
    # 192.168.1.42 on the eth0 network and forward to eth1 and eth2.
    #
    mgroup from eth0 group 225.1.2.3
    mroute from eth0 group 225.1.2.3 source 192.168.1.42 to eth1 eth2
    
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
config file support, reloading config on SIGHUP, and source-less
on-demand (*,G) routing.

Issue tracker and GIT repository available at GitHub:

   * [Repository](http://github.com/troglobit/smcroute)
   * [smcroute-1.99.2.tar.bz2](ftp://troglobit.com/smcroute/smcroute-1.99.2.tar.bz2),
     [MD5](ftp://troglobit.com/smcroute/smcroute-1.99.2.tar.bz2.md5)
   * [Issue Tracker](http://github.com/troglobit/smroute/issues)
   * [Debian packages](http://packages.debian.org/smcroute)
   * [Ubuntu packages](http://packages.ubuntu.com/smcroute)
   * [Google Group](https://groups.google.com/forum/?fromgroups#!forum/smcroute)

See also the [Free(code) page](http://freecode.com/projects/smcoute).

