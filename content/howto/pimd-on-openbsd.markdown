---
name:  "Run pimd on OpenBSD"
title: "HowTo run pimd on OpenBSD"
date: 2015-07-19 00:32:00 +02:00
categories: [ "multicast", "pimd", "openbsd", "bsd", "howto" ]
---

This is an introduction to HowTo run [pimd](/pimd.html) on OpenBSD.  I
keep it around mostly as a reminder to myself when testing new pimd
releases, maybe someone else can make use of it as well.

First of all, my sincere thanks to the [OpenBSD](http://www.openbsd.org)
team for, not just an awesome UNIX distribution, but also for their good
taste in shipping a MULTICAST enabled kernel in the base distribution!
On both NetBSD and FreeBSD [there is a bit of work][2] to get multicast
support, which is one of the reasons for my not writing a HowTo for
either of them atm.

Now, OpenBSD already ships mrouted, which is an older flood and prune based
protocol called DVMRP.  Their version is a few years older than the
[mrouted](/mrouted.html) I maintain.  The DVMRP protocol in mrouted uses a
sort of built-in RIP to figure out how to route multicast.  This was one of
the first things [they](http://tools.ietf.org/html/rfc2362) ripped out when
creating PIM.

PIM stands for *Protocol Independent Multicast*, so you need to run RIP
or OSPF (or BGP or IS-IS) or whatever *unicast* routing protocol first
so that regular traffic works.  Which is kind of useful since you can
test the network with simple `ping` before trying to get any multicast
routing to work ...

<!-- more -->

My topology for testing `pimd` often looks something like the following.
In this HowTo the OpenBSD router is R3:

    .--------.    .----.    .----.    .----.    .----------.
    | Sender |----| R1 |----| R2 |----| R3 |----| Receiver |
    '--------'    '----'    '----'    '----'    '----------'

So, starting out with a clean OpenBSD install we begin by setting up the
basics:

* Activate unicast & multicast routing in the kernel
* Enable ripd and set system up as a multicast router
* Configure ripd
* Download & build pimd
* Run pimd
* Troubleshooting

### Enable Unicast & Multicast Routing in the Kernel

    cp /etc/examples/sysctl.conf /etc/

Uncomment the following two lines

    net.inet.ip.forwarding=1        # 1=Permit forwarding (routing) of IPv4 packets
    net.inet.ip.mforwarding=1       # 1=Permit forwarding (routing) of IPv4 multicast packets


### Enable ripd and set System up as Multicast Router

Create the file /etc/rc.conf.local, unless you already have it.  Add the
following lines to enable ripd and enable the use of a multicast routing
daemon:

    # Enable ripd, also edit /etc/ripd.conf
    ripd_flags=""           # for normal use: ""
    
    # Multicast routing configuration
    # Please look at netstart(8) for a detailed description if you change these
    multicast_host=NO       # Route all multicast packets to a single interface
    multicast_router=YES    # A multicast routing daemon will be run, e.g. mrouted


### Configure ripd

Create the file /etc/ripd.conf by using the template

    cp /etc/examples/ripd.conf /etc/

Edit the file and add interface sections to all the interfaces you want
ripd to operate on, or to not operate on.  Here is my `ripd.conf`:

    fib-update yes
    redistribute connected
    split-horizon poisoned
    triggered-updates yes
    
    # Do not use the WAN interface
    interface pcn0 {
            passive
    }
    
    # This interface is connected to R2
    interface pcn1 {
            cost 1
    }
    
    # This interface is connected to the client/receiver
    interface pcn2 {
            cost 1
    }

Notice how I have `redistribute connected` instead of the default.


### Download & Build pimd

GitHub is the home for pimd development, the latest release is always
available on the [releases][1] page.  For convenience I also provide an
[FTP][4], so it is simple enough to use OpenBSD without any additional
tools.

    $ ftp ftp://ftp.troglobit.com/pimd/pimd-2.3.0.tar.gz
    $ tar xfz pimd-2.3.0.tar.gz
    $ cd pimd-2.3.0/
    $ ./configure; make

You may want to install `pimd` to your system when done, but you can
also run it from the build directory if you like.  The default install
path is `/usr/local/sbin/pimd` and `/etc/pimd.conf`.  Use the `--prefix`
and `--sysconfdir` command line options to the configure script to set
other install directories.  See the file [INSTALL.md][3] for more info.

That's it!


### Run pimd

The default settings in `/etc/pimd.conf` should work for almost all use
cases, but it is good practise to at least disable PIM on interfaces out
to the Internet, or your local office network.  In my case, like in the
case with ripd above, it is `pcn0`.

Uncomment and edit the line `#phyint eth0 disable` in `/etc/pimd.conf`:

    phyint pcn0 disable

Then you can start pimd:

    pimd

To see what it is doing you can run it in the foreground with the debug
command:

    pimd -d

or if you simply want to see the current state.  Notice how it says
`DISABLED` on VIF 0, which is `pcn0`:

    pimd -r
    Virtual Interface Table ======================================================
    Vif  Local Address    Subnet              Thresh  Flags      Neighbors
    ---  ---------------  ------------------  ------  ---------  -----------------
    0  192.168.123.30   192.168.123              1  DISABLED
    1  20.30.0.2        20.30/24                 1  DR PIM     20.30.0.1      
    2  10.141.1.181     10.141.1/24              1  DR NO-NBR
    3  20.30.0.2        register_vif0            1 
    
    Vif  SSM Group        Sources             
    
    Multicast Routing Table ======================================================
    ----------------------------------- (*,G) ------------------------------------
    Source           Group            RP Address       Flags
    ---------------  ---------------  ---------------  ---------------------------
    INADDR_ANY       225.1.2.3        20.30.0.1        WC RP
    Joined   oifs: ....                
    Pruned   oifs: ....                
    Leaves   oifs: ..l.                
    Asserted oifs: ....                
    Outgoing oifs: ..o.                
    Incoming     : .I..                
    
    TIMERS:  Entry    JP    RS  Assert VIFS:  0  1  2  3
    0    40     0       0        0  0  0  0
    ----------------------------------- (S,G) ------------------------------------
    Source           Group            RP Address       Flags
    ---------------  ---------------  ---------------  ---------------------------
    10.142.0.145     225.1.2.3        20.30.0.1        CACHE SG
    Joined   oifs: ....                
    Pruned   oifs: ....                
    Leaves   oifs: ..l.                
    Asserted oifs: ....                
    Outgoing oifs: ..o.                
    Incoming     : .I..                
    
    TIMERS:  Entry    JP    RS  Assert VIFS:  0  1  2  3
    150     5     0       0        0  0  0  0
    --------------------------------- (*,*,G) ------------------------------------
    Number of Groups: 1
    Number of Cache MIRRORs: 1
    ------------------------------------------------------------------------------

You may also want to see what the OpenBSD kernel thinks about the
situation when everything is working:

    # netstat -g
    
    Virtual Interface Table
    Vif  Thresh  Limit  Local-Address    Remote-Address   Pkt_in  Pkt_out
    1       1  20.30.0.2                           8052        0
    2       1  10.141.1.181                           0     8052
    3       1  20.30.0.2                              0        0
    
    Multicast Forwarding Cache
    Hash  Origin           Mcastgroup       Traffic  In-Vif  Out-Vifs/Forw-ttl
    0  10.142.0.145     225.1.2.3             
      1  2/1
    
    Total no. of entries in cache: 1
    
    IPv6 Multicast Interface Table is empty
    IPv6 Multicast Routing Table is empty


### Troubleshooting

The number one problem in multicast routing is the TTL of the multicast
stream from the sender.  In this example I use simple `ping`, but tweak
the TTL and force the output interface:

    root@sender:~# ping -I eth1 -t 10 225.1.2.3           # Linux

The second most common problem is with equipment between the sender or
the receiver and the closes router.  Usually some infrastructure like a
simple switch, or as in the case with virtual setups, a software bridge.
Any one of these semi-intelligent devices can make a mess of your day.
If you suspect trouble, use `ping` from the receiver (or the sender) and
use `tcpdump` on the closest router.  You should be able to see the ICMP
frames.  If not, try disabling *IGMP snooping* on the swithc or software
bridge.  On a Linux *host system* this is done by:

    echo 0 > /sys/devices/virtual/net/virbr*/bridge/multicast_snooping

The third most common problem is actually the firewall in your system.
On Linux systems you get `EPERM` -- "Operation not permitted", and on
OpenBSD you likely get `EHOSTUNREACH` -- "No route to host" all over the
logs, usually for the IGMP traffic that pimd generates.  On OpenBSD this
is due to `pf` by default filtering all frames with IP options set.  I
added support for the "Router Alert" IP option to IGMP frames in v2.2.0,
this is recommended per RFC.   Add this to your `/etc/pf.conf` to allow
IGMP frames to be sent by pimd:

    # Enable IGMP with Router Alert IP option
    pass proto igmp allow-opts


[1]: https://github.com/troglobit/pimd/releases
[2]: /blog/2014/09/23/howto-add-multicast-routing-support-to-the-freebsd-kernel/
[3]: https://github.com/troglobit/pimd/blob/master/INSTALL.md
[4]: ftp://ftp.troglobit.com/pimd/
