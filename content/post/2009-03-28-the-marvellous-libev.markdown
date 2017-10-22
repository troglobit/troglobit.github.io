---
categories: null
comments: true
date: 2009-03-28T23:00:00Z
title: The Marvellous libev
url: /2009/03/28/the-marvellous-libev/
---

A very good friend mine recently told me about a neat event library,
[libev](http://software.schmorp.de/pkg/libev).  Usually when he drops me
links like that it takes me a couple of years to react and finally
adopt.

This time it only took me about a month.

He has actually showed me lots of very useful stuff throughout the
years, and even though we used GNU/Linux at university, he was one
those hard core people who showed me the path into a successful
full-time career as a Linux developer. I don't think I have ever
thanked you properly for that, Jakob!

As it turns out libev was exactly what I have been looking for, for
years!  Way back in 2005 when I switched jobs I started working with a
scary home brew code base ported to Linux from VxWorks.  From the
original code only the basic state machine, basic election algorithms
and some other minor glue code remains.  I have slowly refactored the
code base into something, hopefully, more maintainable.

The one missing piece was the timer implementation.  The old code used
to create a new pthread for each new timeout.  I tried replacing it with
code of
[my own](https://github.com/troglobit/toolbox/blob/master/timer/timer.c),
based on Don Libes
["Implementing Software Timers"](http://www.kohala.com/start/libes.timers.txt).
As it turns out libev already has (working) timers, in fact, it has
taken an even more complete approach and made almost everything into
events with callbacks, perfect for state machines!

After having done
[some initial testing](https://github.com/troglobit/toolbox/blob/master/event-demo.c)
of the libev timers I put on a big smile and fired up Emacs to
investigate what other parts of that home brew code base were eligible
for replacement, when i suddenly hit me: the state machine consists of a
couple of threads that pass messages between them using `msgsnd()` and
`msgrcv()`, bad old XSI IPC primitives.  Bad guys that don't use
standard file descriptors.  A quick and dirty hack confirms it. I
started looking around and I had a vague recollection of some message
primitives that actually used file descriptors.  On the mailing list
Marc (Lehman) was as always extremely quick to answer my questions and
with his help I eventually found the `mq_overview(7)` man page:

> Polling message queue descriptors
>     On Linux, a message queue descriptor is actually a file descriptor,
>     and can be monitored using select(2), poll(2), or epoll(7). This
>     is not portable.

Wow, I could hardly wait until I, later that day, could get my hands on
the keyboard again and try it out!  Yes, the POSIX message queue
primitives work!  Here is my
[sample message queue code](https://github.com/troglobit/toolbox/blob/master/event-demo2.c).
Compile it in Emacs with `M-x compile` (I have that bound to F9 in my
.emacs, Borland IDE style :)

Thus far I must say I can really recommend libev!  Even though it isn't
[ISC licensed](http://www.openbsd.org/policy.html), it's dual licensed
under the
[2-clause BSD license](http://en.wikipedia.org/wiki/BSD_licenses) and
the [GNU GPL](http://www.gnu.org/copyleft/gpl.html).  In my case I'll go
with the 2-clause BSD license, which according to Wikipedia is
"functionally equivalent" to the
[MIT license](http://en.wikipedia.org/wiki/MIT_License).

