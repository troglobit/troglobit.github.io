---
layout: post
title: "Multicast HowTo"
date: 2013-07-09 21:00
comments: true
categories:
- Multicast
- pimd
- mrouted
- SMCRoute
---

The below setup is done using four Ubuntu 12.04 LTS virtual machines
running the linux-virtual kernel package.  In the HowTo I mention both
[pimd](/pimd.html) and [mrouted](/mrouted.html), since they work
out-of-the-box w/o any config changes, but you could just as easily
use [SMCRoute](/smcroute.html) for the same purpose.

When setting up virtual machines and virtual networking there are
several requirements for the host.  The most important one, that needs
pointing out, is a bug in the IGMP snooping code in the Linux bridging
code: the bridge handles the special case 224.0.0.* well, but all
unknown multicast streams outside of that segment should also be
forwarded as-is to all multicast routers.  Since this does not work
with the current IGMP snooping code in the Linux kernel bridge code
you must disable snooping:

    host# echo 0 > /sys/devices/virtual/net/virbr1/bridge/multicast_snooping
    host# echo 0 > /sys/devices/virtual/net/virbr2/bridge/multicast_snooping
    host# echo 0 > /sys/devices/virtual/net/virbr3/bridge/multicast_snooping

Disabling IGMP snooping on the hosts' virbr3 is not really necessary,
but is done anyway for completeness, and also because I re-use the
same setup in other test cases as well.

     R1                       R2                       R3                       R4
    +-------+                +-------+                +-------+                +-------+
    |eth0   | 172.16.12.0/24 |eth0   | 172.16.10.0/24 |eth0   |  10.1.0.0/24   |eth0   |
    |     .1|----------------|.2   .1|----------------|.2   .1|----------------|.2     |
    |   eth1|     virbr1     |   eth1|     virbr2     |   eth1|    virbr3      |       |
    +-------+                +-------+                +-------+                +-------+

This setup can be used in two separate use cases, remember there is only
one multicast routing socket, so you have to choose one of:

   1. pimd -c pimd.conf
   2. mrouted -c mrouted.conf
   3. smcroute -f smcroute.conf

The default configuration files delivered with pimd and mrouted usually
suffice, see their respective manual pages or the comments in each .conf
file for help.

When you start mrouted, you're usually ready to go immediately.  But
in the case of pimd, wait for routers to peer.  Then you can test your
setup using ping from R1 to a tcpdump on R4:

    R1# ping -I eth1 -t 3 225.1.2.3
    R4# tcpdump -i eth0

As soon as the PIM routers R2 and R3 have peered you should start seeing
ICMP traffic reaching R4.

Now, to the actual test case.  The first command for R1 adds a route for
all multicast packets, that is necessary for all tools where you cannot
set the outbound interface for the multicast stream, in our case iperf.
 
    R1# ip route add 224.0.0.0/4 dev eth1
    R1# iperf -u -c 225.1.2.3 -T 3
    R4# iperf -s -u -B 225.1.2.3

The -T option is important since it tells iperf to raise the TTL to 3,
the default TTL for multicast is otherwise 1 due to its broadcast like
nature.

The desired output from iperf is as follows:

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
the multicast routing rules manually.  The by far easiest way to do
this is to update `/etc/smcroute.conf` and and start/restart smcroute,
or send SIGHUP to an already running daemon.  The below example makes
use of the source-less (*,G) approach, since we in our limited setup
have full control over all multicast senders.  There is a slight setup
cost associated with this: the time it takes the kernel to notify
SMCRoute about a new source and before the the actual multicast route
is written to the kernel.  In most cases this is acceptable.

In `smcroute.conf` on R2:
    mgroup from eth0 group 225.1.2.3
    mroute from eth0 group 225.1.2.3 to eth1

In `smcroute.conf` on R3:
    mgroup from eth0 group 225.1.2.3
    mroute from eth0 group 225.1.2.3 to eth1

Now, start smcroute on each of R2 and R4 and then proceed to start
iperf on R4 and R1, as described above. You should get the same result
as with mrouted and pimd.

That's it. Have fun!

FAQ
---

* It doesn't work? -- Check the TTL.
* It doesn't work? -- Check the TTL!
* It doesn't work? -- CHECK THE TTL!
* Why does the TTL in multicast default to 1? -- Because multicast is
  classified as broadcast, which inherently is dangerous.  Without
  proper limitation, like switches with support for IGMP Snooping,
  multicast IS broadcast.
* It doesn't work? -- Check your network, maybe a switch between the
  sender and the receiver doesn't properly support IGMP Snooping.
