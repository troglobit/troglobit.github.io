---
layout: post
title: "Inetd Support in Finit v1.12"
date: 2015-03-06 22:34:45 +0100
comments: true
categories:
- Finit
- inetd
- opensource
---

A steady flow of features, and releases, is key to keeping any project
alive.  Recently I ticked off another item in the [Finit](/finit.html)
TODO list ...

Finit v1.12 now comes with a built-in inetd!  You no longer need an
external inetd daemon to launch services on demand.

The good news doesn't stop there, this little inetd actually supports a
poor man's tcpwrappers!

    inetd ssh/tcp          nowait [2345] /sbin/dropbear -i -R -F
    inetd ssh@eth0:222/tcp nowait [2345] /sbin/dropbear -i -R -F

With these two lines in your `/etc/finit.conf` you tell finit to launch
the [Dropbear SSH](https://matt.ucc.asn.au/dropbear/dropbear.html)
server on demand on port 22 (default ssh/tcp port in `/etc/services`) on
*all* interfaces except on `eth0`, which in your case is the Internet
(WAN) interface, here you want SSH to run on port 222.  Actually, you
don't want port 22 open at all on `eth0` ... so finit takes care of this
for you!  Seriously, it just works, no need for messing about with that
nasty old `iptables` anymore!

The original UNIX inetd super server supported many protocols
internally, some of which may seem a bit odd today, and some have been
superseded by more modern protocols.

* [echo](http://en.wikipedia.org/wiki/Echo_Protocol)
* [chargen](http://en.wikipedia.org/wiki/Character_Generator_Protocol)
* [time](http://en.wikipedia.org/wiki/Time_Protocol)
* [daytime](http://en.wikipedia.org/wiki/Daytime_Protocol)
* [discard](http://en.wikipedia.org/wiki/Discard_Protocol)

Finit currently only supports one internal/built-in standard service,
`time`.  It is built as a plugin to serve as an example of how you can
extend Finit yourself.  The time service can be called either as UDP or
TCP.  To prevent security issues, the `time` protocol is disabled by
default.  To enable it you need two things:

1. The `time.so` plugin (built by default)
2. An `inetd time ...` line in `/etc/finit.conf`

Assuming you've installed the default set of plugins, the following two
lines can be added:

    inetd time/udp   wait [2345] internal
	inetd time/tcp nowait [2345] internal

This can be very useful for testing the inetd capabilities, your network
connection, or simply to get the time to a client where NTP for some
reason does not work, or is blocked.  For instance, you could have a GPS
setup on your server and distribute time to clients with the `time`
protocol.

To use it you need an [`rdate`](http://www.aelius.com/njh/rdate/)
client.  Users of `rdate` in BusyBox may need to be reminded that it
only supports TCP.

    $ rdate -pu 198.51.100.42
	Sat Mar  7 08:48:58 CET 2015

For more info on Finit and its features, see the [README].

Enjoy! ツ

<!-- more -->

### Changes
* Add support for built-in inetd super server -- launch services on
  demand.  Supports filtering per interface and custom Inet ports.
* Upgrade to [libuEv] v1.1.0 to better handle error conditions.
* Allow mixed case config directives in `finit.conf`
* Add support for RFC 868 (rdate) time plugin, start as inetd service.
* Load plugins before parsing `finit.conf`, this makes it possible to
  extend finit even with configuration commands.  E.g., the `time.so`
  plugin must be loaded for the `inetd time/tcp internal` service to be
  accepted when parsing `finit.conf`.
* Slight change in TTY fallback behavior, if no TTY is listed in the
  system `finit.conf` first inspect the `console` setting and only if
  that too is unset fall back to `/bin/sh`
* When falling back to the `console` TTY or `/bin/sh`, finit now marks
  this fallback as console.  Should improve usability in some use cases.

### Fixes
* Revert "Use vfork() instead of fork() before exec()" from v1.11.  It
  turned out to not work so well after all.  For instance, launching
  TTYs in a background process completely blocked inetd services from
  even starting up listening sockets ... proper fork seems to work fine
  though.  This is the casue for *yanking* the [1.11] release.
* Trap segfaults caused by external plugins/callbacks in a sub-process.
  This prevents a single programming mistake in by a 3rd party developer
  from taking down the entire system.
* Fix Coverity CID 56281: `dlopen()` resource leak by storing the
  pointer.  For the time being we do not support unloading plugins.
* Set hostname early, so bootstrap processes like syslog can use it.
* Only restart *lost daemons* when recovering from a `SIGSTOP`/norespawn.

[libuEv]: https://github.com/troglobit/libuev
[1.11]:   https://github.com/troglobit/finit/compare/1.10...1.11
[README]: https://github.com/troglobit/finit/blob/master/README.md
