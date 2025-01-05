---
title: Testing Network Daemons
date: 2025-01-05T13:57:00Z
tags: [ linux, unshare, networking, testing ]
---

As a maintainer of many Open Source projects where networking is key, a
side quest has emerged -- how to test network applications without root?

> The following writeup is part of my series; *Reminders to self*.
> Please let me know if there's anything you'd like me to delve
> further into.

<!--more-->

Depending on your needs, of course, you can use [Qemu][0], and if
needed, connect multiple Qemu instances with [Qeneth][1].  However,
this requires quite a bit of overhead to test a single application.

Enter [Linux namespaces][2], they are the building blocks for what is
more commonly referred to as *containers*.  A container usally employ
all available namespaces to box in one or more applications, but they
also make use of cgroups and various security barriers.  In this blog
post we'll only be looking at namespaces, and in particular *network
namespaces*.

As in programming, a namespace is like a new context, a *carte blanche*
so to speak.  For example, a PID namespace hides all other PIDs in the
system.  Similarily, a *network namespace* hides all the system network
interfaces.  But you can move interfaces between network namespaces, and
that is what we'll make use of here.

### Tools of the Trade

The tools we'll be using are probably already available on your system.
You just don't know about them ... 

 - [unshare][3]
 - [nsenter][4]
 - [ip][5]

Well, the last one you probably should know about if you're an active
Linux user since we've replaced the classic UNIX `ifconfig` tool (and
a few others) with the [iproute2][5] sute.


### Privileged Ports

Running system level services usually means using [privileged ports][6]
so testing such services can be a hassle.  We all want to avoid running
as root, and even `sudo` requires a password (unless you disable it on
your system).  What we want is to be able to run `make check` or similar
in our project to verify that our application works on the standard port
it runs on.

This brings us to our first example use-case for network namespaces:

    $ cd ~/src/my-project/
    $ unshare -mrun
	#

There you go, a root prompt in a dedicated network namespace, meaning
you can now go ahead and start your application :-)

Oh wait, you need one last thing:

	# ip link set lo up state up

Not much networking in UNIX works unless the loopback interfaces is up.
In Linux this brings up the interface, sets the IPv4 and IPv6 loopback 
addresses, and `state up` ensure that we se `UP` instead of `UNKNOWN`
link status.


### LAN Communication

Most projects do well with just the basics, shown in the first example.
However, some may need a bit more, like a proper layer-2 network, i.e.,
a cable between two NICs.  We can emulate that with a VETH pair.

	# ip link add veth0a type veth peer veth0b

This gives us a virtual Ethernet cable with an `a` and a `b` side.  Now,
we need to move one end of that cable into a separate network namespace,
yes another `unshare` inside the first one.  Only problem is that they
need a live process to exist, in our first case this is covered by the
shell we use in the terminal.  Ideally, the process in the new unshare
could be the network daemon we want to test, but to keep things simple
in this example we'll use `sleep`.

    # unshare -run sleep infinity &
	# pid=$!

The PID of the `sleep` process is our handle into the new namespace.
Here's how the `ip link` command can be used to move `veth0b`:

    # ip link set veth0b netns $pid
	# ip -br l
    lo               DOWN           00:00:00:00:00:00 <LOOPBACK> 
    veth0a@if2       DOWN           8a:2f:99:38:a7:6c <BROADCAST,MULTICAST> 

We can peek into the nested unshare using the `nsenter` command:

    # nsenter -t 1646509 -n ip -br l
    lo               DOWN           00:00:00:00:00:00 <LOOPBACK> 
	veth0b@if3       DOWN           fa:e3:0f:a6:dd:77 <BROADCAST,MULTICAST> 

That's it, this is all the tools you need to get started, good luck!

[0]: https://www.qemu.org/
[1]: https://github.com/wkz/qeneth
[2]: https://man7.org/linux/man-pages/man7/namespaces.7.html
[3]: https://man7.org/linux/man-pages/man1/unshare.1.html
[4]: https://man7.org/linux/man-pages/man1/nsenter.1.html
[5]: https://en.wikipedia.org/wiki/Iproute2
[6]: https://www.w3.org/Daemon/User/Installation/PrivilegedPorts.html



