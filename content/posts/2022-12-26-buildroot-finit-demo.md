---
title: Buildroot demo of FastInit (Finit)
date: 2023-09-25T04:48:00Z
orig-date: 2022-12-26T21:21:21Z
tags:
 - finit
 - init
 - opensource
---

I believe there is a **gap in the market** between BusyBox init and
systemd.  In particular in the embedded space.  This blog post shows how
easily it is to get up and running quickly with FastInit ([Finit][1])!

I'm a really bad salesman, and an even worse writer, so instead of
trying to convince you with my poor English, I've made a demo.  It is a
[Buildroot external](https://nightly.buildroot.org/#outside-br-custom)
that can be used to add Finit to your own projects.

> For details, look here: <https://github.com/troglobit/br2-finit-demo>

![Finit bootstrap](/images/finit-demo/bootstrap.png)

<!--more-->

Why Finit?
----------

 - Sane built-in defaults
 - Supports runlevels
 - Respawns your services (customizable)
 - Scriptable nearly everywhere
 - Supports service/task synchronization (called conditions)
 - Supports PID/systemd/s6 readiness notification
 - Supports tmpfiles.d like systemd
 - Supports `ifup -a` & `ifdown -a` natively
 - Supports `/etc/rc.local` and has built-in `runparts` for [SysV init
   compatibility](https://github.com/troglobit/finit/tree/master/doc#sysv-init-compatibility)
 - And [more ...](https://github.com/troglobit/finit#introduction)


Try it Out
----------

First, check your system for the necessary packages to build a buildroot
system <https://buildroot.org/downloads/manual/manual.html#requirement>

 1. Download the demo GIT repo:

        git clone https://github.com/troglobit/br2-finit-demo.git

 2. Change to the top directory and fetch all submodules:

        cd br2-finit-demo/
        git submodule update --init

 3. Configure and Build

        make qemu_x86_64_defconfig
        make

 4. Run

        make run

 5. Log in, user `root`, empty password

![Finit login](/images/finit-demo/login.png)

It boots fairly quick provided you have an x86_64 host computer.  See
the `initctl` tool to control Finit.

    initctl help

Brings up the available commands.  Check what is running right now:

    initctl status   # default command, you can omit 'status'

Show available .conf snippets that start services:

    initctl ls

Try enabling some services (not a lot is enabled in the defconfig, add
more with `make menuconfig` or `make busybox-menuconfig`) like:

    initctl enable telnetd

![Enable service](/images/finit-demo/enable.png)

Try `initctl ls` again, it's now listed in the set of enabled services.
But it's not yet running ... (check with `initctl status`).  This is
because we may want to roll out multiple changes to a system before
activating them.  To activate we tell Finit to reload its configuration.

    initctl reload

Verify it is now running.  For more detailed information about telnetd:

    initctl status telnetd

![Service status](/images/finit-demo/status.png)

Try killing telnetd and check the status again.  You can now see that
Finit has already restarted it for you (`Restarts: 1 (1/10)`).  Kill it
a few more times to see what happens.  When a service reaches its max
restart count Finit will no longer try to restart it automatically --
something is obviously not right with the service.  You will have to
restart it manually, which clears the restart counter.  Notice how the
total restarts counter continues counting.

You can connect to the system from your host, or anywhere on the LAN:

    telnet localhost 8023


Fin
---

![Finit poweroff](/images/finit-demo/poweroff.png)

There is of course a lot more to cover.  Please get in touch with me if
you are curious about some other aspect that would be suitable for a
blog post.  There is also a [discussion forum][2] open for general
questions about Finit.

[1]: https://github.com/troglobit/finit#introduction
[2]: https://github.com/troglobit/finit/discussions
