---
categories:
- FreeBSD
- kernel
- multicast
- HowTo
comments: true
date: 2014-09-23T01:55:19Z
name: MROUTING in FreeBSD
title: 'HowTo: Add Multicast Routing to FreeBSD kernel'
url: /2014/09/23/howto-add-multicast-routing-support-to-the-freebsd-kernel/
aliases: /blog/2014/09/23/howto-add-multicast-routing-support-to-the-freebsd-kernel/
---

This is a very short blog post, mostly intended as a reminder to myself.
Basically, there are two methods of adding multicast routing support to
the FreeBSD kernel:

1. Rebuild the kernel with `options MROUTING`
2. Load the kernel module: `kldload ip_mroute`

The kernel rebuild assumes the `src.txz` set was installed previously.

    cd /usr/src
    cd sys/amd64/conf
    cat GENERIC | sed 's/GENERIC$/MULTICAST/' > MULTICAST
    echo 'options   MROUTING         # Multicast routing' >> MULTICAST
    echo 'options   PIM              # Enable for pimd'   >> MULTICAST
    cd -
    make kernel KERNCONF=MULTICAST
    reboot

That's it.  Remember to make sure your Qemu VM has enough RAM or it
will probably page fault on you.  I use 1,0 GB RAM.

The other option, to load the ready made module, is likely better.  But
you want it to load at boot.  So add this to `/boot/loader.conf`:

    ip_mroute_load="yes"

