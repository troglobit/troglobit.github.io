---
layout: page
title: "Finit | Fast init replacement"
sharing: true
footer: true
date: 2014-11-27 22:36
comments: false
---

<a href="https://github.com/troglobit/finit"><img style="position: absolute; top: 0; right: 0; border: none; box-shadow: none;" src="https://camo.githubusercontent.com/365986a132ccd6a44c23a9169022c0b5c890c387/68747470733a2f2f73332e616d617a6f6e6177732e636f6d2f6769746875622f726962626f6e732f666f726b6d655f72696768745f7265645f6161303030302e706e67" alt="Fork me on GitHub" data-canonical-src="https://s3.amazonaws.com/github/ribbons/forkme_right_red_aa0000.png"></a>

{% img right /images/finit.jpg 456 120 %}

Finit is a small [SysV init](https://en.wikipedia.org/wiki/Init)
replacement with
[process supervision](https://en.wikipedia.org/wiki/Process_supervision)
similar to that of [daemontools](http://cr.yp.to/daemontools.html) and
[runit](http://smarden.org/runit/).  Its focus is on small and
embedded GNU/Linux systems, although fully functional on standard
server and desktop installations.

Finit is fast because it starts services in parallel, it then
*supervises and automatically restarts them if they fail*.  This can
be extended upon with custom callbacks for all services, hooks into
the boot process, or plugins to extend the functionality and adapt
Finit to your needs.

Finit is not only fast, it's arguably one of the easiest to get started
with.  A complete system can be booted with one simple configuration
file, ``/etc/finit.conf``, see below for an example.

{% gist 10648685 %}

{% img /images/finit-screenshot.jpg 615 398 'Finit Screenshot' 'Finit starting a Debian 6.0 system' %}

This is the continuation of the
[original finit](http://helllabs.org/finit/) by Claudio Matsuoka,
which in turn was reverse engineered from syscalls of the ground
breaking
[EeePC fastinit](http://wiki.eeeuser.com/boot_process:the_boot_process)
daemon -- "gaps filled with frog DNA ..."

Issue tracker and GIT repository available at GitHub:

* [Repository](http://github.com/troglobit/finit)
* [NEWS](https://github.com/troglobit/finit/blob/master/NEWS.md)
* [README](https://github.com/troglobit/finit/blob/master/README.md)
* [TODO](https://github.com/troglobit/finit/blob/master/TODO.md)
* [Issue Tracker](http://github.com/troglobit/finit/issues)
* [finit-1.10.tar.xz](ftp://troglobit.com/finit/finit-1.10.tar.xz),
  [MD5](ftp://troglobit.com/finit/finit-1.10.tar.xz.md5)

See also the [Free(code) page](http://freecode.com/projects/finit).


