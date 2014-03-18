---
layout: page
title: "Finit | Fast init replacement"
sharing: true
footer: true
date: 2013-06-07 22:30
comments: false
---

{% img right /images/finit.jpg 456 120 %}

Finit is an extremely fast /sbin/init replacement focusing on small
and embedded GNU/Linux systems.  It is the continuation of the
original finit by [Claudio Matsuoka](http://helllabs.org/finit/) which
was reverse engineered from syscalls of the
[EeePC fastinit](http://wiki.eeeuser.com/boot_process:the_boot_process)
daemon -- gaps filled with frog DNA ...

This modern finit has extensions for service monitoring, multiple TTYs,
one-shot tasks, runlevels and plugins for I/O monitoring, service
callbacks and various hooks to extend and adapt it to your needs.

Finit restores the raw performance of your system and gives you full
control of the ``main()`` loop of ``/sbin/init``!

Issue tracker and GIT repository available at GitHub:

* [Repository](http://github.com/troglobit/finit)
* [NEWS](https://github.com/troglobit/finit/blob/master/NEWS.rst) 
* [README](https://github.com/troglobit/finit/blob/master/README.rst) 
* [Issue Tracker](http://github.com/troglobit/finit/issues)
* [finit-1.8.tar.xz](ftp://troglobit.com/finit/finit-1.8.tar.xz),
  [MD5](ftp://troglobit.com/finit/finit-1.8.tar.xz.md5)

See also the [Free(code) page](http://freecode.com/projects/finit).


