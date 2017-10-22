---
categories:
- Minix
- Editline
comments: true
date: 2010-07-19T02:40:36Z
title: Minix editline moves to GIT
url: /2010/07/19/minix-editline-moves-to-git/
---

A small heads-up, I've migrated the Minix editline project from Bazaar
to GIT.  The new URL for keeping tabs on your favourite free
`readline()` clone is:

> [http://git.vmlinux.org/editline.git/](http://git.troglobit.com/editline.git)

I'm currently working on fixing up the tree and doing some house
cleaning &mdash; including making more stuff configurable &mdash; before
releasing a 1.14.0 later on.

One such item is the integration of libtool with our autoconf friends.
This should make it lot more portable (again) and also help smooth a
merge with other sources for this library.  More on that as well later,
hopefully.
