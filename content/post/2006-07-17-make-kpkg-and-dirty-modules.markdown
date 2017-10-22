---
categories:
- HowTo
- kernel
- Debian
- Ubuntu
comments: true
date: 2006-07-17T22:10:24Z
title: make-kpkg and dirty modules
url: /2006/07/17/make-kpkg-and-dirty-modules/
aliases: /blog/2006/07/17/make-kpkg-and-dirty-modules/
---

Dammit, tonight I spent too many hours chasing down a "feature" in the
Debian kernel build process or the Linux-2.6 kernel.  I haven't yet
deciced who to blame ;-)

Here goes: when you build the latest Linux 2.6 kernel, checked out from
the git repository, with `make-kpkg` you should make sure to uncheck the
`CONFIG_LOCALVERSION_AUTO` option in the kernel config before hand.
It's under "General setup" &rarr; "Automatically append version
information ...".  If you don't disable it, then your `utsrelease.h`
will contain some annoying extra git version string and your modules
directory will look something like this: `/lib/modules/2.6.18-dirty`

All this due to the `LOCALVERSION` config, your git checkout, and the
tiny little fact that `make-kpkg` modifies (moves away) the two files
`scripts/packages/{builddeb,Makefile}` which leads to the modification
of the checkout and the scripts/setlocalversion will tell this to the
top Makefile which in turn will create this whole mess.

Take my advice, untick the `CONFIG_LOCALVERSION_AUTO` option.  It's the
easiest one to get away with.
