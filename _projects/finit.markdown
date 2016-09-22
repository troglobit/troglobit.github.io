---
layout: page
title:
name: Finit
sharing: true
footer: true
date: 2015-12-04 19:39 +0100
comments: false
---
<img class="center" src="/images/finit3.png" style="width: 310px; height: 240px;" alt="Finit: A fast init for Linux" />

<a href="https://github.com/troglobit/finit"><img style="position: absolute; top: 0; right: 0; border: none; box-shadow: none;" src="https://camo.githubusercontent.com/365986a132ccd6a44c23a9169022c0b5c890c387/68747470733a2f2f73332e616d617a6f6e6177732e636f6d2f6769746875622f726962626f6e732f666f726b6d655f72696768745f7265645f6161303030302e706e67" alt="Fork me on GitHub" data-canonical-src="https://s3.amazonaws.com/github/ribbons/forkme_right_red_aa0000.png" /></a>

Finit is a small event based [SysV init][1] replacement with built-in
[process supervision][2] similar to that of its more famous cousins
[daemontools][3] and [runit][4].  *Services are supervised and
automatically restarted if they fail*.

Finit primarily targets small and embedded Linux systems by heavily
reducing the amount of context switches, forks, and calls to external
tools that most other init daemons suffer from.

Finit supports runlevels, process monitoring, and starting/stopping
services on demand -- either with the built-in inetd support, or by
triggering on free-form conditions.  E.g., PID file created/modified,
Netlink events like default route or interfaces coming up/down, similar
to [systemd][7].

Finit can also be extended with plugins to hook into the boot process,
extend the functionality at runtime to fit your specific needs.  See the
[documentation][README] for details.


Example
-------

See [TroglOS][9] for an example of how to boot a small embedded system
with Finit:

<img class="left" src="/images/finit3-screenshot.png" alt="Finit Screenshot" style="width: 472px; height: 422px;">


Configuration
-------------

The following is an example of `/etc/finit.conf`.  It can be split up in
per-service `.conf` files in `/etc/finit.d` to be able to change and
reload the configuration at runtime.

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
    service :1 [2345] <net/eth1/up> /sbin/dropbear -R -F -p 22        -- SSH daemon (LAN)
    service :2  [345] <net/route/default> /sbin/dropbear -R -F -p 222 -- SSH daemon (WAN)
    #service    [2345] /sbin/telnetd -F                               -- Telnet daemon
    
    # Optional network bringup script, automatic on Debian/BusyBox systems
    #network /etc/init.d/networking

    # System patch or extension scripts, see run-parts(8).
    # Can also use /etc/rc.local for smaller things.
    #runparts /mnt/start.d
    
    # Inetd services
	# Allow telnet on standard port only if not from WAN (eth0)
	# Allow telnet onport 2323 from WAN (don't do this kids)
	# Built-in rdate service also available on custom port 3737, notice internal.time
    inetd ftp/tcp                   nowait [2345] /sbin/uftpd -i -f        -- FTP daemon
    inetd tftp/udp                    wait [2345] /sbin/uftpd -i -y        -- TFTP daemon
    inetd time/udp                    wait [2345] internal                 -- UNIX rdate service
    inetd time/tcp                  nowait [2345] internal                 -- UNIX rdate service
    inetd 3737/tcp                  nowait [2345] internal.time            -- UNIX rdate service
    inetd telnet/tcp@*,!eth0,       nowait [2345] /sbin/telnetd -i -F      -- Telnet daemon
    inetd 2323/tcp@eth0             nowait [2345] /sbin/telnetd -i -F      -- Telnet daemon
    #inetd 222/tcp@eth0              nowait [2345] /sbin/dropbear -i -R -F -- SSH service
    #inetd ssh/tcp@*,!eth0           nowait [2345] /sbin/dropbear -i -R -F -- SSH service
    
    # Allow login on ttyUSB0, for systems with no dedicated console port
    tty [12345] /dev/ttyAMA0 115200 vt100
    tty  [2345] /dev/ttyUSB0 115200 vt100
    console /dev/ttyAMA0


Commands
--------

The familiar `shutdown`, `reboot`, `poweroff`, and `halt` commands are
provided by Finit, as is the traditional `telinit` command.  In addition
to that there is also the more modern `initctl` tool:

    ~ $ initctl status -v
    1       running  476     [S12345]   /sbin/watchdog -T 16 -t 2 -F /dev/watchdog
    2       running  477     [S12345]   /sbin/syslogd -n -b 3 -D
    3       running  478     [S12345]   /sbin/klogd -n
    4:1       inetd  0       [2345]     internal time allow *:37
    4:2       inetd  0       [2345]     internal time allow *:37
    4:3       inetd  0       [2345]     internal 3737 allow *:3737
    5:1       inetd  0       [2345]     /sbin/telnetd allow *:23 deny eth0,eth1
    5:2       inetd  0       [2345]     /sbin/telnetd allow eth0:2323,eth2:2323,eth1:2323
    6:1       inetd  0       [345]      /sbin/dropbear allow eth0:222
    6:2       inetd  0       [345]      /sbin/dropbear allow *:22 deny eth0

For details, see the [documentation][README] and the online help `-h` to
each tool.


Origin
------

This project is the continuation of the [original finit][5] by Claudio
Matsuoka, which was reverse engineered from syscalls of the
[EeePC fastinit][6] daemon -- "gaps filled with frog DNA ..."

Issue tracker and GIT repository available at GitHub:

* [Repository](http://github.com/troglobit/finit)
* [CHANGELOG](https://github.com/troglobit/finit/blob/master/CHANGELOG.md)
* [README](https://github.com/troglobit/finit/blob/master/README.md)
* [TODO](https://github.com/troglobit/finit/blob/master/TODO.md)
* [Issue Tracker](http://github.com/troglobit/finit/issues)
* [finit-3.0-rc1.tar.xz](ftp://ftp.troglobit.com/finit/finit-3.0-rc1.tar.xz),
  [MD5](ftp://ftp.troglobit.com/finit/finit-3.0-rc1.tar.xz.md5)

See also the [Free(code) page](http://freecode.com/projects/finit).

[1]: https://en.wikipedia.org/wiki/Init
[2]: https://en.wikipedia.org/wiki/Process_supervision
[3]: http://cr.yp.to/daemontools.html
[4]: http://smarden.org/runit/
[5]: http://helllabs.org/finit/
[6]: http://wiki.eeeuser.com/boot_process:the_boot_process
[7]: https://www.freedesktop.org/wiki/Software/systemd/
[9]: https://github.com/troglobit/troglos
[README]: https://github.com/troglobit/finit/blob/master/README.md

<!--
  -- Local Variables:
  -- mode: markdown
  -- End:
  -->
