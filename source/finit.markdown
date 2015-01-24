---
layout: page
title: "Finit | Fast & Extensible init for Linux"
sharing: true
footer: true
date: 2014-11-27 22:36
comments: false
---

<a href="https://github.com/troglobit/finit"><img style="position: absolute; top: 0; right: 0; border: none; box-shadow: none;" src="https://camo.githubusercontent.com/365986a132ccd6a44c23a9169022c0b5c890c387/68747470733a2f2f73332e616d617a6f6e6177732e636f6d2f6769746875622f726962626f6e732f666f726b6d655f72696768745f7265645f6161303030302e706e67" alt="Fork me on GitHub" data-canonical-src="https://s3.amazonaws.com/github/ribbons/forkme_right_red_aa0000.png"></a>

{% img right /images/finit.jpg 456 120 %}

Finit is a [SysV init][1] replacement with [process supervision][2]
similar to that of [daemontools][3] and [runit][4].  Its focus is on
small and embedded GNU/Linux systems, yet fully functional on standard
server and desktop installations.

Finit is optimized for small embedded systems by heavily reducing the
amount of context switches, forks, and calls needed to external tools.
Services are *supervised and automatically restarted if they fail*.

Finit can be extended with custom callbacks for all services, hooks into
the boot process, or plugins to extend the functionality and adapt your
boot process to fit your needs.

{% gist 10648685 %}

{% img /images/finit-screenshot.jpg 615 398 'Finit Screenshot' 'Finit starting a Debian 6.0 system' %}

This project is the continuation of the [original finit][5] by Claudio
Matsuoka, which was reverse engineered from syscalls of the ground
breaking [EeePC fastinit][6] daemon -- "gaps filled with frog DNA ..."

Issue tracker and GIT repository available at GitHub:

* [Repository](http://github.com/troglobit/finit)
* [CHANGELOG](https://github.com/troglobit/finit/blob/master/CHANGELOG.md)
* [README](https://github.com/troglobit/finit/blob/master/README.md)
* [TODO](https://github.com/troglobit/finit/blob/master/TODO.md)
* [Issue Tracker](http://github.com/troglobit/finit/issues)
* [finit-1.11.tar.xz](ftp://troglobit.com/finit/finit-1.11.tar.xz),
  [MD5](ftp://troglobit.com/finit/finit-1.11.tar.xz.md5)

See also the [Free(code) page](http://freecode.com/projects/finit).

[1]: https://en.wikipedia.org/wiki/Init
[2]: https://en.wikipedia.org/wiki/Process_supervision
[3]: http://cr.yp.to/daemontools.html
[4]: http://smarden.org/runit/
[5]: http://helllabs.org/finit/
[6]: http://wiki.eeeuser.com/boot_process:the_boot_process

