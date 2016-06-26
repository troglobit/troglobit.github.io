---
layout: post
title: "Bugfix release of uftpd"
date: 2014-09-04 22:22:09 +0200
comments: true
categories: 
- opensource
- ftp
- tftp
---

> So them pesky details of `/etc/inetd.conf` really are important?

This is a small bugfix release of [uftpd](/uftpd.html).  Version 1.4
is basically just to change `nowait` to `wait` for the TFTP service in
`/etc/inetd.conf`, but there's also a minor man page update.

Enjoy! :)
