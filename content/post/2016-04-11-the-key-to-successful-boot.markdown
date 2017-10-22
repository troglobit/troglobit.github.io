---
categories:
- UNIX
comments: true
date: 2016-04-11T01:28:27Z
title: The key to successful boot
url: /2016/04/11/the-key-to-successful-boot/
---

How do you know when your UNIX service (daemon) is ready?  Simple, it
has created a PID file, signalling to you how to reach it.  Usually this
file is created as `/var/run/daemon.pid`, or `/run/daemon.pid`, and has
the PID of `daemon` as the first and only data in the file.  This data
may or may not have a UNIX line ending.

Only trouble is: most UNIX daemons do not re-assert that PID file
properly on `SIGHUP` (if they support `SIGHUP` that is).  When I send
`SIGHUP` to a daemon I expect it to re-read its `/etc/daemon.conf` and
resume operation, basically a quicker way than stop/start.

Annoyingly however, most daemons do not signal us back to tell us when
they're done with the `SIGHUP`.  Naturally a new movement has risen that
says we should all instrument our daemons with D-bus ... I say no.
Simply touch the PID file instead.

<!--more-->

Yeah, one could argue the natural (and pure) thing would be to add a
UNIX domain socket and use a `daemonctl` client instead of `SIGHUP` +
PID file ... but for this little mechanism of signalling back to the
user that a daemon is ready for business, it's too much overhead.

My own Init replacement, [Finit](/finit.html), is being fitted with a
system to synchronize services with events.  Eg. wait for one service to
start, an interface to be created, or come up, or have an address set,
or a gateway to be set ... and so on.

In the case where process B depends on process A you do not want to
start process B before process A is actually up and running.  Simply
starting process A is rarely sufficient -- starting B too soon can
lead to B terminating prematurely because it cannot yet connect to A.

One may argue that B should try and reconnect, or that A and B should
have some other means of synchronizing.  Sure, when dependencies are
clear and developers create cooperating services this works great.  But
this is rarely the case in real life.  Services are usually developed by
multiple teams, scattered across both time and distance.  So what we're
left with is finding the least common denominator and use that for our
synchronization needs.

Waiting for daemon A to create its PID file before we start process B is
enough.  When the system is reconfigured -- many services may need to be
restarted -- we can shake the dependency tree and send `SIGHUP` to all
daemons in the correct order.  The only patching required is to ensure
that all daemons re-assert their PID files after having reloaded their
respective config files.

More on the changes in Finit3 and the upcoming new dependency systems
in a later article.  Hopefully this will have made you interested!


