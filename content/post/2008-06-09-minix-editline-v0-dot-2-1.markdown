---
categories:
- opensource
- Editline
comments: true
date: 2008-06-09T23:02:22Z
title: Minix editline v0.2.1
url: /2008/06/09/minix-editline-v0-dot-2-1/
aliases: /blog/2008/06/09/minix-editline-v0-dot-2-1/
---

The v0.2.0 release included some Debian patches, `tcgetattr()` and a
batch mode (when reading from file) line reader.  This release fixes a
bug in the Debian patch that caused the batch mode version of
`readline()` to actually truncate lines longer than 64 chars.

Get it from the usual FTP location:

* http://ftp.vmlinux.org/pub/People/jocke/minix-editline/
