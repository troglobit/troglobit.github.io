---
categories:
- opensource
- ftp
- tftp
comments: true
date: 2014-09-04T22:22:09Z
title: Bugfix release of uftpd
url: /2014/09/04/bugfix-release-of-uftpd/
aliases: /blog/2014/09/04/bugfix-release-of-uftpd/
---

> So them pesky details of `/etc/inetd.conf` really are important?

This is a small bugfix release of [uftpd](/uftpd.html).  Version 1.4
is basically just to change `nowait` to `wait` for the TFTP service in
`/etc/inetd.conf`, but there's also a minor man page update.

Enjoy! :)
