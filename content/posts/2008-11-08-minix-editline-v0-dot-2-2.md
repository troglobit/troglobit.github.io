---
categories:
- opensource
- Editline
comments: true
date: 2008-11-08T20:24:22Z
title: Minix editline v0.2.2
url: /2008/11/08/minix-editline-v0-dot-2-2/
aliases: /blog/2008/11/08/minix-editline-v0-dot-2-2/
---

Oups, it seems I forgot to announce the v0.2.2 release of the Minix
editline library!  It was made official in Bazaar over a month ago,
2008-10-02, but it was not until today that the tarball was created and
uploaded to the FTP.

The most noteworthy in this release is support for command completion
with the addition of `rl_complete()` and `rl_list_possib()`.  Two
function pointers that easily can be overloaded by the user.  See the
examples section of the tree for example usage.

Get it from the usual FTP location:

* http://ftp.vmlinux.org/pub/People/jocke/minix-editline/
