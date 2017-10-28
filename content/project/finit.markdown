---
title: "Fast, extensible init for Linux systems"
date: 2017-10-28 12:04:00 +0200
aliases: /finit.html
---
<img class="center noborder" src="/images/finit3.png" style="width: 310px; height: 240px;" alt="Finit: A fast init for Linux" />

Finit is a small event based [SysV init][1] replacement with built-in
inetd, getty, and [process supervision][2] similar to [systemd][7],
[runit][4] and [daemontools][3] -- *Services are supervised and
automatically restarted if they fail*.

Finit primarily targets small and embedded Linux systems by greatly
reducing the amount of context switches, forks, and calls to external
tools that most other init daemons suffer from.

Finit supports runlevels, process monitoring, and starting/stopping
services on demand -- either with the built-in inetd support, or by
triggering on free-form conditions.  E.g., PID file created/modified,
Netlink events like default route or interfaces coming up/down, similar
to [systemd][7].

Finit can also be extended with plugins to hook into the boot process
and extend its functionality to fit your specific needs.  For more
details, see the [documentation][README].


Example
-------

[TroglOS][9] provides an example of how to boot an embedded system with
Finit, as well as extend it with plugins, or replace any default plugin.
TroglOS use an [mtd.so][10] plugin to handle uninitialized flash:

<img class="center" src="/images/finit3-screenshot.png" alt="Finit Screenshot" style="width: 472px; height: 422px;">


Configuration
-------------

The following is an example of `/etc/finit.conf`.  It can be split up in
per-service `.conf` files in `/etc/finit.d/`, useful for distributions
and to simplify changing service configuration at runtime.

```conf
# /etc/finit.conf - System bootstrap for TroglOS
user root
host default

# Default runlevel
runlevel 2

# Launch bootstrap services
service [S12345] /sbin/watchdogd -nx                              -- System watchdog daemon
service [S12345] /sbin/syslogd -n -b 3 -D                         -- System log daemon
service [S12345] /sbin/klogd -n                                   -- Kernel log daemon

# Services must not daemonize themselves, look for -n, --foreground or
# similar switches to prevent them from forking to the background
#service :1 [2345] <net/eth1/up>       /sbin/dropbear -R -F -p 22  -- SSH daemon (LAN)
#service :2  [345] <net/route/default> /sbin/dropbear -R -F -p 222 -- SSH daemon (WAN)
#service    [2345]                     /sbin/telnetd -F            -- Telnet daemon

# Finit understands /etc/network/interfaces on Debian/BusyBox systems
# on other systems you can use the `network SCRIPT` stanza or a simple
# run or task stanza to run the necessary scripts.
#network /etc/init.d/networking

# System patch or extension scripts, see run-parts(8), built-in support in Finit.
# You can also use /etc/rc.local for smaller things.
#runparts /mnt/start.d

# Inetd services
# Allow telnet on standard port only if not from WAN (eth0)
# Allow telnet onport 2323 from WAN (don't do this kids)
# Built-in rdate service also available on custom port 3737, notice internal.time
inetd ftp/tcp                   nowait [2345] /sbin/uftpd -i -f       -- FTP daemon
inetd tftp/udp                    wait [2345] /sbin/uftpd -i -y       -- TFTP daemon
inetd time/udp                    wait [2345] internal                -- UNIX rdate service
inetd time/tcp                  nowait [2345] internal                -- UNIX rdate service
inetd 3737/tcp                  nowait [2345] internal.time           -- UNIX rdate service
#inetd 2323/tcp@eth0             nowait [2345] /sbin/telnetd -i -F     -- Telnet daemon (WAN)
inetd telnet/tcp@*,!eth0,       nowait [2345] /sbin/telnetd -i -F     -- Telnet daemon (LAN)
inetd 222/tcp@eth0             nowait [2345] /sbin/dropbear -i -R -F -- SSH daemon (WAN)
inetd ssh/tcp@*,!eth0          nowait [2345] /sbin/dropbear -i -R -F -- SSH daemon (LAN)

# Allow login on ttyUSB0, for systems with no dedicated console port
tty [12345] /dev/ttyAMA0 115200 vt100 noclear
tty  [2345] /dev/ttyUSB0 115200 vt100 noclear
console /dev/ttyAMA0
```

Finit configuration files in `/etc/finit.d/` are monitored for changes,
if the `mtime` is changed on a file and the user calls `initctl reload`,
that program is reloaded (`SIGHUP`:ed, or stop-started depending on the
`<!>` in the service declaration.).


Commands
--------

The familiar `shutdown`, `reboot`, `poweroff`, and `halt` commands are
provided by Finit, as is the traditional `telinit` command.  In addition
to that there is also the more modern `initctl` tool:

```sh
~ $ initctl status -v
1       running  476     [S12345----]  /sbin/watchdog -T 16 -t 2 -F /dev/watchdog
2       running  477     [S12345----]  /sbin/syslogd -n -b 3 -D
3       running  478     [S12345----]  /sbin/klogd -n
4:1       inetd  0       [--2345----]  internal time allow *:37
4:2       inetd  0       [--2345----]  internal time allow *:37
4:3       inetd  0       [--2345----]  internal 3737 allow *:3737
5:1       inetd  0       [--2345----]  /sbin/telnetd allow *:23 deny eth0,eth1
5:2       inetd  0       [--2345----]  /sbin/telnetd allow eth0:2323,eth2:2323,eth1:2323
6:1       inetd  0       [---345----]  /sbin/dropbear allow eth0:222
6:2       inetd  0       [---345----]  /sbin/dropbear allow *:22 deny eth0
```

For details, see the [documentation][README] and the online help `-h` to
each tool.


Origin
------

This project is the continuation of the [original finit][5] by Claudio
Matsuoka, which was reverse engineered from syscalls of the
[EeePC fastinit][6] daemon -- "gaps filled with frog DNA ..."

Issue tracker and GIT repository available at GitHub:

* [Repository](http://github.com/troglobit/finit)
* [ChangeLog](https://github.com/troglobit/finit/blob/master/ChangeLog.md)
* [README][]
* [TODO](https://github.com/troglobit/finit/blob/master/TODO.md)
* [Issue Tracker](http://github.com/troglobit/finit/issues)
* [finit-3.0.tar.xz](ftp://ftp.troglobit.com/finit/finit-3.0.tar.xz),
  [MD5](ftp://ftp.troglobit.com/finit/finit-3.0.tar.xz.md5)

See also the [Free(code) page](http://freecode.com/projects/finit).

[1]: https://en.wikipedia.org/wiki/Init
[2]: https://en.wikipedia.org/wiki/Process_supervision
[3]: http://cr.yp.to/daemontools.html
[4]: http://smarden.org/runit/
[5]: http://helllabs.org/finit/
[6]: http://wiki.eeeuser.com/boot_process:the_boot_process
[7]: https://www.freedesktop.org/wiki/Software/systemd/
[9]: https://github.com/troglobit/troglos
[10]: https://github.com/troglobit/troglos/blob/master/packages/finit/plugins/mtd.c
[README]: https://github.com/troglobit/finit/blob/master/README.md
