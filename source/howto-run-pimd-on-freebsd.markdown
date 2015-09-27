---
layout: page
title: "howto run pimd on freebsd"
date: 2015-09-27 18:45
comments: true
sharing: true
footer: true
---

This is not a proper HowTo, but more of a "note to self".  For these
notes the following virtual topology, running on Ubuntu 15.10 with
Linux 4.2.

    .--------.    .----.    .----.    .----------.
    | Sender |----| R2 |----| R3 |----| Receiver |
    '--------'    '----'    '----'    '----------'

Here only R2 and R3 run FreeBSD v10.2, 64-bit.  The `Sender<-->R2` and
`R3-Receiver` networks are both setup using DHCP on the Linux bridges.
Only the `R2<-->R3` network is setup statically, using `20.30.0.0/24`.

## Setup

You need a few things set up in FreeBSD before starting `pimd`.

In `/boot/loader.conf` add the following line to load the MROUTING
kernel module:

    ip_mroute_load="yes"

In `/etc/rc.conf` there are several lines that need to be added, both to
enable router/gateway mode and to enable RIP to resolve the unicast
forwarding table.  The `Sender` and `Recieiver` nodes, which run
Ubuntu 14.04, employ Quagga `ripd` for this, but on FreeBSD we employ
the god 'ol `routed` :-)

    gateway_enable="YES"
	routed_enable="YES"
	routed_flags="-s"

Notice how we change the `routed_flags` from quiet mode! Now, for
interop with the rest of the network we enable RIP v2 mode by adding the
following line to `/etc/gateways`:

    ripv2

## Download

Next we need to download and build `pimd`.  It is available on GitHub if
you want the latest bleeding edge stuff, or use the latest tarball:

    ftp ftp://troglobit.com/pimd/pimd-2.3.0.tar.gz
	tar xfz pimd-2.3.0.tar.gz
	cd pimd-2.3.0/
	./configure
	make

## Running

Now we can start it on both routers:

    ./pimd -c pimd.conf

After a while you should be able to see some interesting output in both

    ./pimd -r

and

    netstat -rn

See the [OpenBSD HowTo](/howto-run-pimd-on-openbsd.html) for example
output.
