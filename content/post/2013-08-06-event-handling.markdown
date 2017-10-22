---
categories:
- Projects
- opensource
- SMCRoute
- Threads vs Events
comments: true
date: 2013-08-06T00:00:00Z
title: Threads vs Events
url: /2013/08/06/event-handling/
aliases: /blog/2013/08/06/event-handling/
---

This is a rant about something I recently found to be a long standing
battle line in the world of programming,
[Lau78](http://cgi.di.uoa.gr/~mema/courses/mde518/papers/lauer78.pdf).
The event vs thread based approach to programming.  As rants go I do
not aspire to deliver a clear or logical message, what so ever.  It's
basically just something I need to get off my chest.

It was not until 2007 I first learned about the event based approach
to programming and event libraries like
[libevent](http://libevent.org/) and
[libev](http://software.schmorp.de/pkg/libev.html).  Up until that
point the silver bullet everyone was using was ... Threads.

<!--more-->

I don't really know when it all started, maybe it was the Linux
revolution, the first NPTL release with GLIBC, Java or Solaris.
Nevertheless, from my point of view it was sometime in the mid 90's
during my time at university that the use of threads was starting to
become prevalent.

<p data-pullquote='"With the rise of the thread based model of
programming we now had a hammer, and every problem looked like a
nail."'>I had a gut feeling there was something really wrong with using
threads for every conceivable program, but I could not find a way to
express it, so I chugged away with my threads, semaphores and condition
variables.  I convinced myself I was happy like this.</p>

Of course I knew about the event based approach, but it was more or
less dismissed as a thing of the past, a while(1) loop to mimic the
behavior of PLC's.  So almost every program I wrote, and every program
I took over from others, were like Indiana Jones types of mazes full
of deadlocks and race conditions.

I thought I did something wrong, and so did many others like me.  I
spent days and nights trying to understand, refactor, and redesign
threaded programs.  What I found was a doubt that the thread based
model actually didn't suit every problem,
[Ous96](http://www.cc.gatech.edu/classes/AY2009/cs4210_fall/papers/ousterhout-threads.pdf).
There are quite a few domains, however, where thread based models
shine. Usually in languages that come with thread support built-in,
like Erlang.

Most of the programs I work with today are network daemons.  Meaning
they are essentially message based applications that spend a lot of
time waiting for an event to occur: receiving a data frame, waiting
for a timer to expire, a signal to be raised, etc.  Of course threads
can be used for this, but it is a lot simpler to employ an event based
framework instead.  Also, they are all written in C for speed and
portability between different UNIX systems.  For that domain, where I
currently make my living, it will be difficult to convince me to ever
look at threads again.

