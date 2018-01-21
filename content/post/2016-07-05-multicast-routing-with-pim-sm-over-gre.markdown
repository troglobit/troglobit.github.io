---
categories:
- UNIX
- multicast
- PIM
comments: true
date: 2016-07-05T22:04:36Z
title: Multicast routing with PIM-SM over GRE
url: /2016/07/05/multicast-routing-with-pim-sm-over-gre/
aliases: /blog/2016/07/05/multicast-routing-with-pim-sm-over-gre/
---

GRE tunnels are useful in many ways.  This blog post shows how to set up
multicast routing with [pimd](https://github.com/troglobit/pimd/) over a
GRE tunnel.  To achieve this we will also set up OSPF over GRE with
[Quagga](http://www.quagga.net), because PIM, unlike DVMRP (`mrouted`),
require unicast routing rules to be established.

            .----{ Intranet }----.
           /    192.168.1.0/24    \
          /                        \
     .10 /                          \.20
     .--'---. .1  GRE Tunnel  .2 .---`--.
     |      |====================|      |
     |  R1  |   172.16.16.0/30   |  R2  |
     |      |                    |      |
     '--.---'                    '------'
        | .1                        | .1 
        |    10.0.1.0/24            |    10.0.2.0/24
        | .2                        | .2 
     .--'---.                    .--'---.
     |      |                    |      |
     |  C1  |                    |  C2  |
     |      |                    |      |
     '------'                    '------'

In this post we are using the home WiFi network, 192.168.1.0/24, to hook
up the GRE tunnel.  It is just as easy to extend this to a big corporate
Intranet with more routers between `R1` and `R2`.  As long as that IT
department takes care of the unicast routing between `R1` and `R2` so
that the GRE tunnel can be established.

<!--more-->

Now, on router R1 we set up the first GRE tunnel endpoint:

    ip tunnel add gre0 mode gre remote 192.168.1.20 local 192.168.1.10 ttl 64
    ip link set gre0 multicast on
    ip link set gre0 up
    ip addr add 172.16.16.1/30 dev gre0

We do not add any static route for `R1` to reach the LAN on `R2` that
`C2` is connected to, that is for OSPF to add dynamically for us later.
Notice, hower, that we must explicitly enable the multicast flag on the
GRE interface, it is not enabled by default in Linux.

On router `R2` we can now set up the other side of the GRE tunnel:

    ip tunnel add gre0 mode gre remote 192.168.1.10 local 192.168.1.20 ttl 64
    ip link set gre0 multicast on
    ip link set gre0 up
    ip addr add 172.16.16.2/30 dev gre0

Setup of OSPF in Debian or Ubuntu distributions is only an `apt-get`
away followed by enabling the zebra and ospf daemons:

    sudo apt-get install quagga
    sudo editor /etc/quagga/daemons

The idea is to set up an OSPF backbone, area 0, for our routers without
wrecking havoc in the big corporate intranet, which may already run OSPF
... so OSPF should only talk on the `gre0` interface, and maybe even the
LAN interfaces towards `C1` and `C2` (in case we want to expand on this
example later).  In our setup the routers use `wlan0` to connect to the
intranet.  We can use the sample configuration files to start from:

    sudo cp /usr/share/doc/quagga/examples/zebra.conf.sample /etc/quagga/zebra.conf
    sudo cp /usr/share/doc/quagga/examples/ospfd.conf.sample /etc/quagga/ospfd.conf

The `zebra.conf` can be left as-is, just edit the `ospfd.conf` to look
like this:

    hostname ospfd
    password zebra
    router ospf
        passive-interface wlan0
        redistribute connected
        network 172.16.16.0/30 area 0
	
When the routers have peered, the two clients `C1` and `C2` should be
able to (unicast) ping each other.  Telnet into the OSPF daemon using
`telnet localhost ospfd` and type `show ip ospf neigh` to see all OSPF
neighbors and their status, should be `Full/...` when done exchanging
routes.  Use `show ip ospf route` to see the exchanged routes, also
inspect the kernel routing table with `route -n`.  Use `traceroute` to
confirm the traffic between clients do traverse the GRE tunnel and not
over the Intranet.

	root@C1:~$ traceroute 10.0.2.2
	traceroute to 10.0.2.2 (10.0.2.2), 30 hops max, 60 byte packets
	 1  10.0.1.1 (10.0.1.1)  0.427 ms  0.384 ms  0.309 ms
	 2  172.16.16.2 (172.16.16.2)  3.135 ms  4.724 ms  5.786 ms
	 3  10.0.2.2 (10.0.2.2)  9.979 ms  10.777 ms  10.676 ms

Time for `pimd` to be started.  Just like OSPF we want `pimd` to avoid
talking on `wlan0`, so add the following to the default `pimd.conf`,
which can otherwise be left as-is:

    phyint wlan0 disable

In our case the routers have many interfaces, so we disable *all*
interfaces using the `-N` switch to `pimd` and instead only enable the
interfaces we are intrested in, the GRE tunnel and the client LAN
interface, `eth0`:

    phyint eth0 enable
    phyint gre0 enable

Make sure to add the `phyint` setting to the correct place in the file.
Now start pimd on each router, use the debug mode and run in foreground
first to see what happens:

    root@R1:~$ pimd -N -f -d
    debug level 0xffffffff (dvmrp_detail,dvmrp_prunes,dvmrp_routes,dvmrp_neighbors,dvmrp_timers,igmp_proto,igmp_timers,igmp_members,trace,timeout,packets,interfaces,kernel,cache,rsrr,pim_detail,pim_hello,pim_register,pim_join_prune,pim_bootstrap,pim_asserts,pim_cand_rp,pim_routes,pim_timers,pim_rpf)
    02:01:49.616 pimd version 2.3.2 starting ...
    02:01:49.617 Got 262144 byte send buffer size in 0 iterations
    02:01:49.617 Got 262144 byte recv buffer size in 0 iterations
    02:01:49.617 Got 262144 byte send buffer size in 0 iterations
    02:01:49.617 Got 262144 byte recv buffer size in 0 iterations
    02:01:49.617 Getting vifs from kernel
    02:01:49.618 /etc/pimd.conf:0 - Skipping interface lo, either loopback or does not support multicast.

... and on `R2`:
	
    root@R2:~$ pimd -N -f -d
    debug level 0xffffffff (dvmrp_detail,dvmrp_prunes,dvmrp_routes,dvmrp_neighbors,dvmrp_timers,igmp_proto,igmp_timers,igmp_members,trace,timeout,packets,interfaces,kernel,cache,rsrr,pim_detail,pim_hello,pim_register,pim_join_prune,pim_bootstrap,pim_asserts,pim_cand_rp,pim_routes,pim_timers,pim_rpf)
    02:01:49.616 pimd version 2.3.2 starting ...
    02:01:49.617 Got 262144 byte send buffer size in 0 iterations
    02:01:49.617 Got 262144 byte recv buffer size in 0 iterations
    02:01:49.617 Got 262144 byte send buffer size in 0 iterations
    02:01:49.617 Got 262144 byte recv buffer size in 0 iterations
    02:01:49.617 Getting vifs from kernel
    02:01:49.618 /etc/pimd.conf:0 - Skipping interface lo, either loopback or does not support multicast.

To verify multicast routing works we start a multicast sender on `C1`
and a receiver (sink) on `C2`:

    root@C1:~$ mcjoin -s -t 10
    root@C2:~$ mcjoin
	.................o

Notice the TTL adjustment (`-t 10`) on the sender side, this is needed
because the default TTL for multicast, due to safety concerns, is 1.

The log on the routers should look something like this (snippet):

    01:44:16.750 accept_group_report(): igmp_src 10.0.2.2 ssm_src 0.0.0.0 group 225.1.2.3 report_type 34
    01:44:16.750 Set delete timer for group: 225.1.2.3
    01:44:16.750 Adding vif 4 for group 225.1.2.3
    01:44:16.877 Received IGMP v3 Membership Report from 172.16.16.1 to 224.0.0.22
    01:44:16.877 accept_membership_report(): IGMP v3 report, 40 bytes, from 172.16.16.1 to 224.0.0.22 with 4 group records.
    
    Virtual Interface Table ======================================================
    Vif  Local Address    Subnet              Thresh  Flags      Neighbors
    ---  ---------------  ------------------  ------  ---------  -----------------
      0  192.168.1.123    192.168.1                1  DISABLED
      1  172.17.0.1       172.17                   1  DISABLED
      2  192.168.122.1    192.168.122              1  DISABLED
      3  172.16.16.2      172.16.16/30             1  DR PIM     172.16.16.1
      4  10.0.2.1         10.0.2                   1  DR NO-NBR
      5  172.16.16.2      register_vif0            1 
    
     Vif  SSM Group        Sources             
    
    
    Multicast Routing Table ======================================================
    ----------------------------------- (*,G) ------------------------------------
    Source           Group            RP Address       Flags
    ---------------  ---------------  ---------------  ---------------------------
    INADDR_ANY       225.1.2.3        10.0.2.1         WC RP
    Joined   oifs: ......              
    Pruned   oifs: ......              
    Leaves   oifs: ....l.              
    Asserted oifs: ......              
    Outgoing oifs: ....o.              
    Incoming     : .....I              
    
    TIMERS:  Entry    JP    RS  Assert VIFS:  0  1  2  3  4  5
                 0    45     0       0        0  0  0  0  0  0
    ----------------------------------- (S,G) ------------------------------------
    Source           Group            RP Address       Flags
    ---------------  ---------------  ---------------  ---------------------------
    192.168.64.2     225.1.2.3        10.0.2.1         SPT CACHE SG
    Joined   oifs: ......              
    Pruned   oifs: ......              
    Leaves   oifs: ....l.              
    Asserted oifs: ......              
    Outgoing oifs: ....o.              
    Incoming     : ...I..              
    
    TIMERS:  Entry    JP    RS  Assert VIFS:  0  1  2  3  4  5
               200    55     0       0        0  0  0  0  0  0
    --------------------------------- (*,*,G) ------------------------------------
    Number of Groups: 1
    Number of Cache MIRRORs: 1
    ------------------------------------------------------------------------------
    
    Candidate Rendezvous-Point Set ===============================================
    RP address       Incoming  Group Prefix        Priority  Holdtime
    ---------------  --------  ------------------  --------  ---------------------
    10.0.2.1         5         224/4               20        70      
    10.0.1.1         3         224/4               20        55      
    169.254.0.1      1         232/8               1         65535   
    ------------------------------------------------------------------------------
    Current BSR address: 10.0.2.1

In normal operation, i.e. without the debug flag `-d`, the routing table
and other useful PIM information can be queried from the running `pimd`
by calling it in client mode.  The following is a wrapper for sending a
`SIGUSR1` signal to the daemon and reading `/var/run/pimd/pimd.dump`:

    root@R2:~$ pimd -r
    Virtual Interface Table ======================================================
    Vif  Local Address    Subnet              Thresh  Flags      Neighbors
    ---  ---------------  ------------------  ------  ---------  -----------------
      0  192.168.1.123    192.168.1                1  DISABLED
      1  172.17.0.1       172.17                   1  DISABLED
      2  192.168.122.1    192.168.122              1  DISABLED
      3  172.16.16.2      172.16.16/30             1  DR PIM     172.16.16.1    
      4  10.0.2.1         10.0.2                   1  DR NO-NBR
      5  172.16.16.2      register_vif0            1 
    
     Vif  SSM Group        Sources             
    
    Multicast Routing Table ======================================================
    ----------------------------------- (*,G) ------------------------------------
    Source           Group            RP Address       Flags
    ---------------  ---------------  ---------------  ---------------------------
    INADDR_ANY       225.1.2.3        10.0.2.1         WC RP
    Joined   oifs: ......              
    Pruned   oifs: ......              
    Leaves   oifs: ....l.              
    Asserted oifs: ......              
    Outgoing oifs: ....o.              
    Incoming     : .....I              
    
    TIMERS:  Entry    JP    RS  Assert VIFS:  0  1  2  3  4  5
                 0    20     0       0        0  0  0  0  0  0
    ----------------------------------- (S,G) ------------------------------------
    Source           Group            RP Address       Flags
    ---------------  ---------------  ---------------  ---------------------------
    10.0.1.2         225.1.2.3        10.0.2.1         SPT CACHE SG
    Joined   oifs: ......              
    Pruned   oifs: ......              
    Leaves   oifs: ....l.              
    Asserted oifs: ......              
    Outgoing oifs: ....o.              
    Incoming     : ...I..              
    
    TIMERS:  Entry    JP    RS  Assert VIFS:  0  1  2  3  4  5
               185    10     0       0        0  0  0  0  0  0
    --------------------------------- (*,*,G) ------------------------------------
    Number of Groups: 1
    Number of Cache MIRRORs: 1
    ------------------------------------------------------------------------------

You can also verify that `pimd` actually sets routes in the kernel
multicast routing table with:

    root@R2:~$ ip mroute
    (10.0.1.2, 225.1.2.3)        Iif: gre0       Oifs: eth0


Caveats
-------

There are quite a few caveats to watch out for with multicast routing.
Here are a few:

1. Check the TTL of the multicast stream, must be >1 to be routed.  Rule
   of thumb: increase with each router in topology.
2. Check the MTU of the interfaces in the path.
3. Check the reverse path, make sure you have unicast connectivity
   between end nodes -- PIM requires this to work!
4. Check the multicast sender's source IP, verify that it's not a
   unroutable link-local (169.254) address.
5. Have the PIM routers peered?  Should list 'PIM' and a neighbor IP
   in the output of `pimd -r`.  If NO-NBR or DISABLED is shown you
   have a network or `pimd.conf` problem.
6. As of this writing `pimd` is a bit sensitive to topology changes.
   See [issue #79](https://github.com/troglobit/pimd/issues/79) for
   details and possible future resolution.

For other questions, ideas on setting up multicast routing, see the
general [Multicast HowTo](/multicast-howto.html), or file a bug report
at [GitHub](https://github.com/troglobit)

That's it, good luck!

<!--
  -- Local Variables:
  -- mode: markdown
  -- End:
  -->
