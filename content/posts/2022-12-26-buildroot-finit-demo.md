---
title: Buildroot demo of FastInit (Finit)
date: 2022-12-26T21:21:21Z
lastmod: 2024-01-07T16:48:00Z
tags:
 - buildroot
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

> For details, look here: <https://github.com/troglobit/br2-finit-demo>,
> try out the [latest build][3] in [Qemu][4].

{{% figure src="/images/finit-demo/bootstrap.png" caption="Finit bootstrap" class="center" width=600 %}}

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

    ```shell
    ~$ git clone https://github.com/troglobit/br2-finit-demo.git
    ```

 2. Change to the top directory and fetch all submodules:

    ```shell
    ~$ cd br2-finit-demo/
    ~/br2-finit-demo(main)$ git submodule update --init
    ```

 3. Configure and Build

    ```shell
    ~/br2-finit-demo(main)$ make qemu_x86_64_defconfig
    ~/br2-finit-demo(main)$ make
    ```

 4. Run

    ```shell
    ~/br2-finit-demo(main)$ make run
    ```

 5. Log in, user `root`, empty password

{{% figure src="/images/finit-demo/login.png" caption="Finit login prompt" class="center" width=550 %}}

It boots fairly quick provided you have an x86_64 host computer.  See
the `initctl` tool to control Finit.

```shell
# initctl help
```

Brings up the available commands.  Check what is running right now:

```shell
# initctl status   # default command, you can omit 'status'
```

Show available .conf snippets that start services:

```shell
# initctl ls
```

Try enabling some services (not a lot is enabled in the defconfig, add
more with `make menuconfig` or `make busybox-menuconfig`) like:

```shell
# initctl enable telnetd
```

{{% figure src="/images/finit-demo/enable.png" caption="Enable service" class="center" width=600 %}}

Try `initctl ls` again, it's now listed in the set of enabled services.
But it's not yet running ... (check with `initctl status`).  This is
because we may want to roll out multiple changes to a system before
activating them.  To activate we tell Finit to reload its configuration.

```shell
# initctl reload
```

Verify it is now running.  For more detailed information about telnetd:

```shell
# initctl status telnetd
```

{{% figure src="/images/finit-demo/status.png" caption="Service status" class="center" width=400 %}}

Try killing telnetd and check the status again.  You can now see that
Finit has already restarted it for you (`Restarts: 1 (1/10)`).  Kill it
a few more times to see what happens.  When a service reaches its max
restart count Finit will no longer try to restart it automatically --
something is obviously not right with the service.  You will have to
restart it manually, which clears the restart counter.  Notice how the
total restarts counter continues counting.

You can connect to the system from your host, or anywhere on the LAN:

```shell
~$ telnet localhost 8023
```


Fin
---

{{% figure src="/images/finit-demo/poweroff.png" caption="Finit poweroff" class="center" width=350 %}}

There is of course a lot more to cover.  Please get in touch with me if
you are curious about some other aspect that would be suitable for a
blog post.  There is also a [discussion forum][2] open for general
questions about Finit.

[1]: https://github.com/troglobit/finit#introduction
[2]: https://github.com/troglobit/finit/discussions
[3]: https://github.com/troglobit/br2-finit-demo/releases/tag/latest
[4]: https://www.qemu.org
