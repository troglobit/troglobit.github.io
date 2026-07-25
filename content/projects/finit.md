---
title: "Fast init for Linux systems"
date: 2024-01-07 19:47:00 +0100
lastmod: 2026-07-25 08:10:00 +0200
aliases: [ /finit.html, /finit ]
tags: [ finit, init, opensource ]
image: /images/finit-card.png
description: "Finit is a fast, simple init and process supervisor for embedded, server, and desktop Linux -- an alternative to SysV init and systemd, reverse engineered from the EeePC fastinit."
---
{{% figure src="/images/finit3.png" class="right-floated" link="https://github.com/finit-project/finit/releases/latest" %}}

Finit is an alternative to [SysV init][1] and [systemd][7], originally
reverse engineered from the EeePC fastinit by Claudio Matsuoka — "gaps
filled with frog DNA …"

> [Latest release][20] available on [GitHub][19]


Features
--------

  * Runlevels, defined per service
  * One-shot tasks, services (daemons), or [SysV init][1] start/stop scripts
  * Runparts and `/etc/rc.local` support
  * Process supervision similar to [systemd][7]
  * Sourcing environment files
  * Conditions for network/process/custom dependencies
  * Pre/Post script actions
  * Tooling to enable/disable services
  * Optional built-in getty
  * Optional built-in watchdog, with support for hand-over to [watchdogd](https://troglobit.com/watchdogd.html)
  * Built-in support for Debian/BusyBox `/etc/network/interfaces`, automatically calls ifup/ifdown
  * Cgroups v2, both configuration and monitoring in `initctl top`
  * Plugin support for customization
  * Proper rescue mode with bundled `sulogin` for protected maintenance shell (optional)


Used By
-------

The [GitHub finit/contrib/ section][contrib], also part of the release
tarball, include sample configurations for Debian, Void, and Alpine
Linux.

{{% figure src="/images/alpine-screenshot2.png" class="center" caption="Alpine Linux v3.19 with Finit v4.7" %}}

The following [Buildroot][17] derivatives provide examples of how to
boot embedded systems with Finit:

  - [Infix OS][16]
  - [myLinux][9]
  - [Westermo NetBox][11]

There's even a [br2-external demo][18] available, with a [downloadable
image][21], which may be more accessible to beginners.

On the desktop and server side, [Finix][finix], by Aaron Andersen,
builds a full Linux distribution around Finit.


Configuration
-------------

Finit reads its setup from `/etc/finit.conf` and, in the recommended
"split layout", one `foo.conf` file per service in `/etc/finit.d/`.
Available but not-yet-enabled services live in `/etc/finit.d/available`
and are turned on with the `initctl` tool.  A small taste:

```ApacheConf
# Monitor and (re)start these services as needed
service [S12345] syslogd -n -b 3 -D              -- System log daemon
service [S12345] watchdogd -L -f                 -- HW watchdog daemon
service   [2345] sshd -D                         -- OpenSSH daemon

# Wait for a default route before starting NTP, log stdout/stderr
service [2345] log <net/route/default> ntpd -n -l -I eth0 -- NTP daemon

# A getty on the first VT and the serial console
tty [12345] /dev/tty1  linux
tty [12345] @console   noclear
```

The `service` stanza, as well as `task`, `run` and others, share the
same shape:

```
service [LVLS] <COND> log env:[-]/etc/default/daemon daemon ARGS -- Daemon daemon
^       ^      ^      ^   ^                          ^              ^       ^
|       |      |      |   |                          |              |        `-- Optional description
|       |      |      |   |                          |              `----------- Daemon arguments
|       |      |      |   |                          `-------------------------- Path to daemon
|       |      |      |    `---------------------------------------------------- Optional env. file
|       |      |       `-------------------------------------------------------- Redirect output to log
|       |       `--------------------------------------------------------------- Optional conditions
|        `---------------------------------------------------------------------- Optional Runlevels
 `------------------------------------------------------------------------------ Monitored application
```

Runlevels, conditions and the description are all optional, so simple
cases stay simple:

    service /usr/sbin/sshd -D

Dependencies are expressed with [conditions][conditions].  Here Finit waits for
basic networking before starting nginx, and stops it again if the
default route goes away:

    service <net/route/default> nginx -- High performance HTTP server

At runtime the `initctl` tool queries status and starts, stops, and
reloads services.

**Note:** make sure daemons *do not* fork and detach from the
controlling TTY, usually an `-n`, `-f`, or `-D` flag.  If a daemon
detaches, Finit cannot monitor it and will try to restart it.


Blog Posts
----------

For more on the features above, see the following posts:

  * [Finit v4.2 -- Try before you buy!](/post/2022-01-16-finit-v4.2-released/)
  * [Dependency handling in Finit](/post/2021-05-01-dependency-handling-in-finit/)
  * [It's here! ♥](/post/2021-04-27-its-here/) -- Finit v4 release post
  * [Alpine Linux with Finit](/post/2021-02-12-alpine-linux-with-finit/)
  * [Buildroot demo of FastInit (Finit)](/post/2022-12-26-buildroot-demo-of-fastinit-finit/)


Documentation
-------------

This page is just a taste.  Finit now has thorough, well-organized
documentation covering the full boot sequence, every stanza, and each
plugin:  **<https://finit-project.github.io>**

  - [Configuration][config] — every stanza in detail
  - [Conditions][conditions] — dependency handling
  - [Commands & Status][initctl] — the `initctl` tool
  - [Plugins][plugins] — hooks and I/O extensions
  - [Signals][signals] — reboot, halt, and signal handling


Origin
------

This project is the continuation of the [original finit][5] by Claudio
Matsuoka, which was reverse engineered from syscalls of the
[EeePC fastinit][6] daemon.

Please file bug reports, clone it, or send pull requests for bug fixes
and proposed extensions using GitHub:

* [Repository][19]
* [Issue Tracker](https://github.com/finit-project/finit/issues)
* [README][]
* [TODO](https://github.com/finit-project/finit/blob/master/TODO.md)
* [ChangeLog](https://github.com/finit-project/finit/blob/master/ChangeLog.md)

[1]: https://en.wikipedia.org/wiki/Init
[5]: http://helllabs.org/finit/
[6]: http://wiki.eeeuser.com/boot_process:the_boot_process
[7]: https://www.freedesktop.org/wiki/Software/systemd/
[9]: https://github.com/troglobit/myLinux
[11]: https://github.com/westermo/netbox
[16]: https://github.com/kernelkit/infix
[17]: https://buildroot.org
[18]: https://github.com/troglobit/br2-finit-demo
[19]: https://github.com/finit-project/finit
[20]: https://github.com/finit-project/finit/releases/latest
[21]: https://github.com/troglobit/br2-finit-demo/releases/tag/latest
[README]: https://github.com/finit-project/finit/blob/master/README.md
[contrib]: https://github.com/finit-project/finit/tree/master/contrib
[config]: https://finit-project.github.io/config/
[conditions]: https://finit-project.github.io/conditions/
[initctl]: https://finit-project.github.io/initctl/
[plugins]: https://finit-project.github.io/plugins/
[signals]: https://finit-project.github.io/signals/
[finix]: https://github.com/finix-community/finix/
