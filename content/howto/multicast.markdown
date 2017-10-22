---
title: "Multicast HowTo"
date: 2015-07-16 15:42:00 +02:00
aliases: [/multicast-howto.html]
categories: [ "multicast", "pimd", "mrouted", "SMCRoute", "IGMP", "igmpproxy", "mcproxy" ]
---

This HowTo attempts to give some insight into the basics of setting up
multicast routing.  Both static multicast routing, with [SMCRoute][1],
and dynamic multicast routing, with [mrouted][2] and [pimd][3].

For some use-cases it may not be possible to use multicast routing, then
I recommend trying out:

* [igmproxy][4], [mcproxy][5], or
* OpenVPN in Layer-2, [bridged mode][6]

Make sure to check out the [FAQ](#faq) for the most common problems.

Good Luck! <br />
-- Joachim


Basics
------

To understand multicast routing one needs to first understand how
multicast on a LAN works.  In this HowTo the LAN is usually referred to
as Layer-2 and routing (between LANs) as Layer-3.

### Layer-2 vs Layer-3

First, without any form of regulation multicast is broadcast, which is
inherently bad.  We don't want to route broadcast, but even on a LAN we
want to keep broadcast to a minimum.  Before switches we had hubs which
caused *all* traffic to be broadcast.  We can do better, not just for
the sake of security but also to increase bandwidth.

Enter IGMP and MLD, the control protocol for multicast on a LAN, for
IPv4 and IPv6 respectively.  Essentially they act as a subscription
based protocol, every N seconds sending out a Query to all nodes on the
LAN; "Anyone want multicast?", to which a node can reply; "Yes please,
I'd like to have 225.1.2.3.".  MLD (IPv6) works in a similar way.

Most managed switches today support *IGMP Snooping* which is just a
fancy way of saying: this switch does not flood multicast like broadcast
on all ports.  However, it also means that everyone on a LAN that want
multicast have to be able to request it.

### Multicast Distribution Point

On a LAN with many IGMP Snooping capable switches a *Querier election*
will take place.  The switch (or router) with the lowest IP address
wins, unless that address is 0.0.0.0.  The elected IGMP querier on a LAN
becomes the *distribution point* for all multicast.  All other switches
on the LAN must forward both known and unknown multicast to the Querier.

Usually the network IP plan (design) is set in such a way that the
router has the lowest IP address on the LAN, so that it will always
be the elected IGMP querier, hence receiving all multicast so it can
route it as required.

### Multicast Router Ports

In a network with redundant/multiple multicast routers one cannot rely
solely on the IGMP querier election.  Instead, most managed switches
have a setting called *Multicast Router Ports* where you can configure
on which ports to *also* flood all multicast.

This feature can also be used to forward multicast to nodes that do not
speak IGMP/MLD.  But since it will forward all multicast it is usually
better to set up static FDB entries per multicast group on the switch
instead.

> Many switches are limited to filtering multicast based on the
> *multicast MAC* equivalent.  In our case of 225.1.2.3 it would be
> mapped to 01:00:5e:01:02:03.  For more on this, and the limitations
> it brings, see [RFC1112](https://www.ietf.org/rfc/rfc1112.txt).

### PIM-SM :: IGMP v2 vs. PIM-SSM :: IGMP v3

In the beginning there was darkness and DVMRP conquered the earth.  The
God of all multicast, which is Steve Deering, was pleased.  Then light
came upon us, gods were overthrown and PIM-SM was invented.

The story continues but becomes a bit of a blur because so much happened
in such a short time frame.  There are RFCs that tell the tale better, go
read up on them.  What eventually came out of it was this:

IGMP v2 was an OK protocol, a client requested a group, 225.1.2.3, and
it was unblocked by switches leading up to the Querier and multicast was
received.

PIM-SM was OK, many routers could agree on what groups each of them had,
set up a distribution tree with magic distribution point/routers,
called rendez-vous points (RPs), for one or more, but not necessarily
all multicast groups.  This could be tweaked by hand as well to optimize
distribution.

But what if we had multiple senders for the same group?  A ridiculous
thought at first, but as deployments grew a need for optimizing also
based on the sender (source) grew as well.  Enter PIM-SSM for layer-3
and IGMP v3 on layer-2.  The biggest change is the ability to forward
multicast based on source *and* group, called (S,G).  An end node on a
LAN could now send an IGMP report requesting (192.168.10.1, 225.1.2.3).


### What is TTL and Why Can't I Route Multicast?

The single most common problem with routing multicast that everybody
runs into is the TTL.

The TTL is the the *Time To Live* field in the IP header of a frame.

    $ tcpdump -i lo -vvv icmp
    tcpdump: listening on lo, link-type EN10MB (Ethernet), capture size 262144 bytes
    20:36:50.146377 IP (tos 0x0, ttl 1, id 0, offset 0, flags [DF], proto ICMP (1), length 84)
        192.168.122.1 > 225.1.2.3: ICMP echo request, id 16746, seq 20, length 64
    20:36:51.146380 IP (tos 0x0, ttl 1, id 0, offset 0, flags [DF], proto ICMP (1), length 84)
        192.168.122.1 > 225.1.2.3: ICMP echo request, id 16746, seq 21, length 64

The TTL for broadcast and multicast is by default 1, which means that a
router should not forward the frame beyond the originating LAN.  Again,
without any regulation multicast is broadcast and we do not want to
route broadcast!

The singular best way to fix this problem is for the sender to to set a
higher TTL.  Set it only as high as the number of hops you want this to
be forwarded!

> Multicast is used for A LOT of protocols, many of which was only ever
> intended to be either link-local or limited to the LAN.  Check with
> the appropriate standard (RFCs are freely available online) before
> attempting something foolish that may cause unintended side effects.

For such cases when you want to connect two remote sites and make them
into one big LAN you might be better off using, e.g. a bridged SSL VPN
on layer-2.

However, when you absolutely cannot change the TTL at the sender and
bridging the two LANs is out of the question, then you can try using
the firewall.  On Linux systems you can *mangle* matching frames with
the following magic rule(s):

    iptables -t mangle -A PREROUTING -d 225.1.2.3 -j TTL --ttl-inc 1

or, if the sender runs on a system with Linux you can change the frame
as it egresses:

    iptables -t mangle -A OUTPUT -d 225.1.2.3 -j TTL --ttl-set 128

The group can also be a range, so `239.0.0.0/8` is possible to enter,
and as is shown in these examples, either `--ttl-set` or `--ttl-inc` can
be used to adjust the TTL value.


CORE
----

These days I do most of my multicast testing with [CORE][7], which is
readily available i Debian/Ubuntu as simple as:

    sudo apt-get install core-gui

CORE is very simple to get started with:

1. Fire up the GUI
2. Drag and drop a few router icons to the grid
3. Connect them
4. *BOOM* you now have IP addresses automatically assigned!
5. Press the Play button -- routers are now starting up

Play around a bit to try it out, it's awesome!  I usually start `pimd`,
`mrouted`, and `smcroute` manually with a script.  (You can access the
shell of each router by right-clicking on them.)  The host file system
is reachable from each router in CORE, even though they are isolated and
have their own [network namespaces][8].

For more info on network simulation, like the equally awesome [GNS3][9]
for instance, see [Brian Linkletter's Reviews][10].
   
<a name="roll-your-own"></a>

Roll your own Cloud
-------------------

The below setup is done using four Ubuntu 12.04 LTS virtual machines
running the linux-virtual kernel package.  In the HowTo I mention both
[pimd](/pimd.html) and [mrouted](/mrouted.html), since they work
out-of-the-box w/o any config changes, but you could just as easily
use [SMCRoute](/smcroute.html) for the same purpose.

When setting up virtual machines and virtual networking there are many
pitfalls and several requirements for the host to consider.  The most
important one, that needs pointing out, is a *bug in the IGMP snooping
code* in the Linux bridging code: the bridge handles the special case
`224.0.0.*` well, but all unknown multicast streams outside of that
segment should also be forwarded as-is to all multicast routers.  Since
this does not work with the current IGMP snooping code in the Linux 3.13
kernel bridge code you must disable snooping:

    host# echo 0 > /sys/devices/virtual/net/virbr1/bridge/multicast_snooping
    host# echo 0 > /sys/devices/virtual/net/virbr2/bridge/multicast_snooping
    host# echo 0 > /sys/devices/virtual/net/virbr3/bridge/multicast_snooping

Disabling IGMP snooping on the hosts' `virbr3` is not really necessary,
but is done anyway for completeness, and also because I re-use the
same setup in other test cases as well.

> It is of course not recommended to disable IGMP snooping on a bridge,
> but if it's buggy you really don't have a choice.  Please check this
> for yourself since it depends on the kernel you run.

```
    R1                        R2                        R3                        R4
.--------.                .--------.                .--------.                .--------.
|eth0    | 172.16.12.0/24 |eth0    | 172.16.10.0/24 |eth0    |  10.1.0.0/24   |eth0    |
|      .1|----------------|.2    .1|----------------|.2    .1|----------------|.2      |
|    eth1|     virbr1     |    eth1|     virbr2     |    eth1|    virbr3      |        |
'--------'                '--------'                '--------'                '--------'
```

This setup, or
[another more advanced](ftp://ftp.troglobit.com/pimd/lab-network.svg),
can be used for trying out SMCRoute, pimd and mrouted.  Remember there
is only one multicast routing socket, so for each router (Rn) you have
to choose one of:

1.  `pimd -c pimd.conf`
2.  `mrouted -c mrouted.conf`
3.  `smcroute -f smcroute.conf`

The default configuration files delivered with `pimd` and `mrouted`
usually suffice, see their respective manual pages or the comments in
each `.conf` file for help.

When you start `mrouted`, you're usually ready to go immediately.  But
in the case of `pimd`, wait for routers to peer.  Then you can test your
setup using multicast `ping` from R1 to a `tcpdump` on R4:

    R1# ping -I eth1 -t 3 225.1.2.3
    R4# tcpdump -i eth0

As soon as the PIM routers R2 and R3 have peered you should start seeing
ICMP traffic reaching R4.  If you don't, then check the underlying
routing protocol, e.g. RIP or OSPF, to make sure the *reverse path* is
known -- this is required for PIM since, unlike DVMRP (`mrouted`) which
sort of has RIP built-in, PIM relies on a unicast routing protocol to
have populated the routing table.

Now, to the actual test case.  The first command for R1 adds a route for
all multicast packets, that is necessary for all tools where you cannot
set the outbound interface for the multicast stream, in our case
`iperf`.
 
    R1# ip route add 224.0.0.0/4 dev eth1
    R1# iperf -u -c 225.1.2.3 -T 3
    R4# iperf -s -u -B 225.1.2.3

The `-T` option is important since it tells `iperf` to raise the TTL to
3, the default TTL for multicast is otherwise 1 due to its broadcast
like nature.

The desired output from `iperf` is as follows:

    R1# iperf -u -c 225.1.2.3 -T 3
    ------------------------------------------------------------
    Client connecting to 225.1.2.3, UDP port 5001
    Sending 1470 byte datagrams
    Setting multicast TTL to 3
    UDP buffer size:  160 KByte (default)
    ------------------------------------------------------------
    [  3] local 172.16.12.1 port 55731 connected with 225.1.2.3 port 5001
    [ ID] Interval       Transfer     Bandwidth
    [  3]  0.0-10.0 sec  1.25 MBytes  1.05 Mbits/sec
    [  3] Sent 893 datagrams

    R4# iperf -s -u -B 225.1.2.3
    ------------------------------------------------------------
    Server listening on UDP port 5001
    Binding to local address 225.1.2.3
    Joining multicast group  225.1.2.3
    Receiving 1470 byte datagrams
    UDP buffer size:  160 KByte (default)
    ------------------------------------------------------------
    [  3] local 225.1.2.3 port 5001 connected with 172.16.12.1 port 55731
    [ ID] Interval       Transfer     Bandwidth        Jitter   Lost/Total Datagrams
    [  3]  0.0-10.0 sec  1.25 MBytes  1.05 Mbits/sec   0.268 ms    0/  893 (0%)

To achieve the same using [SMCRoute](/smcroute.html) you need to setup
the multicast routing rules manually.  The easiest way to do this is to
update `/etc/smcroute.conf` and and start/restart `smcroute`, or send
`SIGHUP` to an already running daemon.  The below example makes use of
the source-less (*,G) approach, since we in our limited setup have full
control over all multicast senders.  There is a slight setup cost
associated with this: the time it takes the kernel to notify SMCRoute
about a new source and before the the actual multicast route is written
to the kernel.  In most cases this is acceptable.

In `smcroute.conf` on R2:

    mgroup from eth0 group 225.1.2.3
    mroute from eth0 group 225.1.2.3 to eth1

In `smcroute.conf` on R3:

    mgroup from eth0 group 225.1.2.3
    mroute from eth0 group 225.1.2.3 to eth1

Now, start `smcroute` on each of R2 and R4 and then proceed to start
iperf on R4 and R1, as described above. You should get the same result
as with `mrouted` and `pimd`.

That's it. Have fun!


FAQ
---

1. It doesn't work?

   Check the TTL.

2. Why does the TTL in multicast default to 1?

   Because multicast is classified as broadcast, which inherently is
   dangerous.  Without proper limitation, like switches with support for
   IGMP Snooping, multicast IS broadcast.

3. I cannot change the TTL of the multicast sender, what can I do?!

   Ouch, then you may have to use some firewall mangling technique.
   Here is how you could do it on Linux with iptables:

        iptables -t mangle -A PREROUTING -d GROUP[/LEN] -j TTL --ttl-set 64

   Where `GROUP` is the multicast group address of the stream you want
   to change the TTL of, with an optional prefix length `LEN` if you want
   to specify a range of groups.  From this [RedHat mailing list entry][11].

4. I want to use the loopback interface, but it doesn't show in `pimd`?

   Some interfaces, like `lo` or `tunN`, do not have the `MULTICAST`
   interface flag set by default.  It should work if you enable it:

        ip link set lo multicast on
        ip link set tun1 multicast on

5. It doesn't work?

   Check your network topology, maybe a switch between the sender and
   the receiver doesn't properly support IGMP snooping.

   For virtual/cloud setups, see above for disabling IGMP snooping
   entirely in the Linux kernel.

6. The PIM routers seem to have peered, and they list the multicast
   groups I want to forward, the TTL is OK, but I see no traffic?

   Could be your underlying routing protocol (RIP/OSPF) does not know
   the reverse path to the source.  Make sure the sender's network is
   listed in the routing table on the receiving sides routing table.

7. What's the routing performance of `pimd`/`mrouted`/`smcroute`?

   N/A.  Neither of them take active part in the actual forwarding of
   multicast frames.  This is what the kernel or dedicated routing HW
   does.  The routing daemons `pimd`/`mrouted`/`smcroute` only
   manipulate the multicast routing table(s) of the operating system's
   kernel.

8. Do you have any example of how to set up GRE and `pimd`?

   Yes, for all the gory details see
   [this howto](/blog/2016/07/05/multicast-routing-with-pim-sm-over-gre/)

[1]: https://github.com/troglobit/smcroute/
[2]: https://github.com/troglobit/mrouted/
[3]: https://github.com/troglobit/pimd/
[4]: http://sourceforge.net/projects/igmpproxy/
[5]: https://github.com/mcproxy/mcproxy
[6]: https://community.openvpn.net/openvpn/wiki/OpenVPNBridging
[7]: http://www.nrl.navy.mil/itd/ncs/products/core
[8]: http://blog.scottlowe.org/2013/09/04/introducing-linux-network-namespaces/
[9]: http://www.gns3.com/
[10]: http://www.brianlinkletter.com/open-source-network-simulators/
[11]: http://www.redhat.com/archives/linux-cluster/2007-September/msg00150.html

<!--
  -- Local Variables:
  -- mode: markdown
  -- End:
  -->
