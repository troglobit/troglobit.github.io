---
layout: page
name:  "Run pimd on FreeBSD"
title: "HowTo run pimd on FreeBSD"
date: 2015-09-27 18:45:00 +02:00
comments: true
sharing: true
footer: true
---

This is not a proper HowTo, more of a "note to self" after having
created a setup to test pimd [issue #57][issue].  For these notes the
following virtual topology, running on Ubuntu 15.10 with Linux 4.2 and
Qemu 2.3.0, is used:

    .--------. net1 .----. net2 .----. net3 .----------.
    | Sender |------| R2 |------| R3 |------| Receiver |
    '--------'      '----'      '----'      '----------'

The networks between the boxes are actually Linux bridge devices (br),
on which you may have to [disable IGMP/MLD snooping][igmp] to get `pimd`
to run smoothly.  Only R2 and R3 run FreeBSD v10.2, 64-bit.  The `net1`,
between sender and `R2`, and `net3`, between `R3` and the receiver, are
both setup using simple DHCP client.  The Linux bridges have a DHCP
server running.  Only `net2` between the two routers is static, using
`20.30.0.0/24`.


## FreeBSD Setup

You need a few things set up in FreeBSD before starting `pimd`.

In `/boot/loader.conf` add the following line to load the MROUTING
kernel module:

    ip_mroute_load="yes"

If you want to serial console access to your FreeBSD, or run FreeBSD in
Qemu (virt-manager), you may want to add the following lines as well to
`/boot/loader.conf`:

    boot_multicons="YES"
    boot_serial="YES"
    comconsole_speed="115200"
    console="comconsole,vidconsole"

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


## Download pimd

Next we need to download and build `pimd`.  It is available on GitHub if
you want the latest bleeding edge stuff, or use the latest tarball:

    ftp ftp://ftp.troglobit.com/pimd/pimd-2.3.0.tar.gz
    tar xfz pimd-2.3.0.tar.gz
    cd pimd-2.3.0/
    ./configure
    make


## Running pimd

Now we can start pimd on both routers:

    ./pimd -c pimd.conf

After a while you should be able to see some interesting output in both

    ./pimd -r

and

    netstat -rn

See the [OpenBSD HowTo](/howto-run-pimd-on-openbsd.html) for example
output and troubleshooting.

[issue]: https://github.com/troglobit/pimd/issues/57
[igmp]:  /multicast-howto.html#roll-your-own
