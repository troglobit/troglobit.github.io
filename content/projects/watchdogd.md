---
name: watchdogd
title: "Advanced watchdog daemon for Linux"
date: 2021-04-30 17:02:00 +01:00
lastmod: 2024-01-02 14:57:00 +01:00
aliases: /watchdogd.html
---

<img style="float: right" src="/images/watchdog.png" alt="http://toonclips.com/design/788"
     title="Watch Dog Detective Taking Notes">

[watchdogd(8)][] is an advanced system and process supervisor primarily
intended for embedded Linux and server systems.  It supports "kicking"
multiple watchdog timer (WDT) devices and can also monitor critical
system resources, supervise the heartbeat of processes[^1] and record
process deadline transgressions.

> Read more about [Built-in Monitors][] in the extended documentation.

The configuration determines how the daemon acts on monitored resources
and supervised processes.  See [watchdogd.conf(5)][] for how to set up
watermarks and trigger external scripts to suit your particular setup.

[^1]: Instrumentation of the processes is needed using the libwdog API,
  🕮 <https://codedocs.xyz/troglobit/watchdogd/wdog_8h.html>

The project is hosted at GitHub, see the following links for more
information, how to download, and build:

 * [Repository](http://github.com/troglobit/watchdogd)
 * [Issue Tracker](http://github.com/troglobit/watchdogd/issues)
 * [ChangeLog](https://github.com/troglobit/watchdogd/blob/master/ChangeLog.md)
 * [README](https://github.com/troglobit/watchdogd/blob/master/README.md)
 * [Features](https://github.com/troglobit/watchdogd/blob/master/doc/features.md)
 * [Advanced Usage](https://github.com/troglobit/watchdogd/blob/master/advanced.md)
 * [TODO](https://github.com/troglobit/watchdogd/blob/master/docs/TODO.md)


[watchdogctl(1)]:    https://man.troglobit.com/man1/watchdogctl.1.html
[watchdogd(8)]:      https://man.troglobit.com/man8/watchdogd.8.html
[watchdogd.conf(5)]: https://man.troglobit.com/man5/watchdogd.conf.5.html
[Built-in Monitors]: https://github.com/troglobit/watchdogd/blob/master/doc/features.md#built-in-monitors
