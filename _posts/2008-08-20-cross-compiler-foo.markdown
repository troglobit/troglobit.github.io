---
layout: post
title: "Cross Compiler Foo"
date: 2008-08-20 20:14:42 +0200
comments: true
categories: 
- Embedded
---

There is a certain magic surrounding cross compilers and the people that
know how to build one.  Not unlike that of (Linux/BSD) kernel
developers.  At work we today support two embedded Linux targets, both
are ARM based, and in neither of the two have we built the cross
compiler ourselves.  The first was ye' old 2.95 based from
[uClinux.org][1] and the second we had a consultant build for us.  Lame!

Ever since I was appointed software architect I've had this nagging
sensation about our cross-compiler situation.  We do everything else:
roll our own archs for Linux, patch BusyBox, design our own L2/L3
network daemons, and what-not.  Very annoying that we have such poor
control of the mantle piece of our build environment.

Sure, it's almost indistinguishable from magic, but it's not hard.
There are fine helpers such as [buildroot][2], ptxdist and the aging
crosstool scripts by Dan Kegel.  I used to do some work with Dan's
scripts in a couple of previous jobs, so it was a pleasant surprise to
find that the project had found a new maintainer!

I can highly recommend the [crosstool-NG][3] project!  It makes building
a toolchain really easy.  In a snap had I copied our uClibc `.config`
into the work dir of my configuration, issued the "build" command and
wham, there it was a coffee break later.  A working GCC v4.2.4 cross
compiler for Arm Xscale (big-endian) with built-in uClibc (no more GLIBC
madness and separate uClibc builds in our tree), not to mention an all
you can eat buffe of extra tools for the target: `strace`, `gdb` &
`gdbserver`, `libdmalloc`, `ncurses`&nbsp;... crazy.

I'll start rolling it out on monday in our department and keep close
tabs on the development of crosstool-NG to be able to grab the
latest 4.3.2, or later GCC when it enters the ct-ng Subversion
repository.

[1]: http://www.uclinux.org/
[2]: http://buildroot.uclibc.org/
[3]: http://crosstool-ng.org/
