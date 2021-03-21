---
categories:
- opensource
- Editline
- Minix
comments: true
date: 2008-06-07T21:45:05Z
title: Minix editline v0.1.0
url: /2008/06/07/minix-editline-v0-dot-1-0/
aliases: /blog/2008/06/07/minix-editline-v0-dot-1-0/
---

I've been looking long and hard for a small and useful [GNU readline][1]
replacement.  Oddly enough, all the time I was looking for one I never
even once considered looking at the [Minix][2] sources!

Currently I'm using the NetBSD [editline derivative][4] (readline
compatible) from [Jess Thrysoee][5], but it requires ncurses, which is
huge!

All I really wanted was a bare bones `readline()` suitable for small
embedded systems.  One which could record history and preferably, but
not required to, support completion.

OK, so yesterday I found the Minix editline implementation, written
years ago by Simmule Turner and Rich Salz. My hat is off to you guys,
you rock!

Here's my first packaged version for Linux:

* http://ftp.vmlinux.org/pub/People/jocke/minix-editline/ 

All I have done, so far, is to move around the files a bit and add the
standard [GNU configure and build system][6].  It now builds and runs
perfectly on my laptop, next step is to cross-build it for a couple of
embedded ARM targets.

I do all development using [GNU Bazaar][7]. If you want to join in, then
branch from the following public URL:

     bzr branch http://vmlinux.org/jocke/bzr/minix-editline 

Patches and ideas are welcome!

[1]: http://www.gnu.org/software/readline/
[2]: http://www.minix3.org/
[4]: http://thrysoee.dk/editline/
[5]: http://thrysoee.dk/
[6]: http://mij.oltrelinux.com/devel/autoconf-automake/
[7]: http://www.bazaar-vcs.org/
