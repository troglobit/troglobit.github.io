---
layout: page
title: "Fast, extensible init for Linux systems"
sharing: true
footer: true
date: 2015-11-23 13:23 +0100
comments: false
---

<a href="https://github.com/troglobit/finit"><img style="position: absolute; top: 0; right: 0; border: none; box-shadow: none;" src="https://camo.githubusercontent.com/365986a132ccd6a44c23a9169022c0b5c890c387/68747470733a2f2f73332e616d617a6f6e6177732e636f6d2f6769746875622f726962626f6e732f666f726b6d655f72696768745f7265645f6161303030302e706e67" alt="Fork me on GitHub" data-canonical-src="https://s3.amazonaws.com/github/ribbons/forkme_right_red_aa0000.png"></a>

{% img right /images/finit.jpg 456 120 %}

Finit is a small event based [SysV init][1] replacement with built-in
[process supervision][2] similar to that of its more famous cousins
[daemontools][3] and [runit][4].  *Services are supervised and
automatically restarted if they fail*.

Finit targets small and embedded Linux systems by heavily reducing the
amount of context switches, forks, and calls to external tools that most
other init daemons suffer from.

Finit supports basic runlevels, process monitoring, and can launch
services on demand -- either with the built-in inetd support, or by
triggering on Linux Netlink events like `IFUP`, `IFDN` or `GW:UP`.

Finit can also be extended with custom callbacks for all services, hooks
into the boot process, or plugins to extend the functionality and adapt
the boot process to fit your needs.  See the [README][] for details.

``` apache
    # /etc/finit.conf - System bootstrap for TroglOS
    user root
    host default
    
    # Default runlevel
    runlevel 2

    # Launch bootstrap services
    service [S12345] /sbin/watchdogd -L -f                      -- System watchdog daemon
    service [S12345] /sbin/syslogd -n -b 3 -D                   -- System log daemon
    service [S12345] /sbin/klogd -n                             -- Kernel log daemon
    
    # Services must not daemonize themselves, look for --foreground or
    # similar switches to standard services.
    #service [2345] /sbin/inetd -f                               -- Internet super daemon
    service :1 [2345] <!IFUP:eth0,GW:UP> /sbin/dropbear -R -F -p 22 -- SSH daemon
    #service :2 [345] /sbin/dropbear -R -F -p 222                -- SSH daemon
    #service [2345] /sbin/telnetd -F                             -- Telnet daemon
    
    # Network bringup script
    network /etc/init.d/networking
    
    # System patch or extension scripts, see run-parts(8).
    runparts /mnt/start.d
    
    # Inetd services
    inetd ftp/tcp                   nowait [2345] /sbin/uftpd -i -f       -- FTP daemon
    inetd tftp/udp                    wait [2345] /sbin/uftpd -i -y       -- TFTP daemon
    inetd time/udp                    wait [2345] internal                -- UNIX rdate service
    inetd time/tcp                  nowait [2345] internal                -- UNIX rdate service
    inetd 3737/tcp                  nowait [2345] internal.time           -- UNIX rdate service
    inetd telnet/tcp@*,!eth1,!eth0, nowait [2345] /sbin/telnetd -i -F     -- Telnet daemon
    inetd 2323/tcp@eth1,eth2,eth0   nowait [2345] /sbin/telnetd -i -F     -- Telnet daemon
    #inetd 222/tcp@eth0              nowait [2345] /sbin/dropbear -i -R -F -- SSH service
    #inetd ssh/tcp@*,!eth0           nowait [2345] /sbin/dropbear -i -R -F -- SSH service
    
    # Allow login on ttyUSB0, for systems with no dedicated console port
    tty [12345] /dev/ttyAMA0 115200 vt100
    tty [12345] /dev/ttyUSB0 115200 vt100
    console /dev/ttyAMA0
```

{% img /images/finit-screenshot.jpg 676 709 'Finit Screenshot' %}

See [TroglOS][9] for an example of how to boot a small embedded system
with Finit.

----

This project is the continuation of the [original finit][5] by Claudio
Matsuoka, which was reverse engineered from syscalls of the
groundbreaking [EeePC fastinit][6] daemon -- "gaps filled with frog DNA
..."

Issue tracker and GIT repository available at GitHub:

* [Repository](http://github.com/troglobit/finit)
* [CHANGELOG](https://github.com/troglobit/finit/blob/master/CHANGELOG.md)
* [README][]
* [TODO](https://github.com/troglobit/finit/blob/master/TODO.md)
* [Issue Tracker](http://github.com/troglobit/finit/issues)
* [finit-2.2.tar.xz](ftp://troglobit.com/finit/finit-2.2.tar.xz),
  [MD5](ftp://troglobit.com/finit/finit-2.2.tar.xz.md5)

See also the [Free(code) page](http://freecode.com/projects/finit).

[1]: https://en.wikipedia.org/wiki/Init
[2]: https://en.wikipedia.org/wiki/Process_supervision
[3]: http://cr.yp.to/daemontools.html
[4]: http://smarden.org/runit/
[5]: http://helllabs.org/finit/
[6]: http://wiki.eeeuser.com/boot_process:the_boot_process
[9]: https://github.com/troglobit/troglos
[README]: https://github.com/troglobit/finit/blob/master/README.md
