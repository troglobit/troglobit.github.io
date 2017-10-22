---
categories:
- opensource
- ftp
- tftp
- ddns
comments: true
date: 2014-09-13T19:18:56Z
title: GnuTLS support in Inadyn and fixes to uftpd
url: /2014/09/13/gnutls-support-in-inadyn-and-fixes-to-uftpd/
---

Quite a few changes lately.  I finally got around to adding support
for [GnuTLS](http://www.gnutls.org) to [Inadyn](/inadyn.html),
hopefully this will get into Debian ... unless the Jessie freeze
prevents that.

Also, thanks to a friend of mine trying out [uftpd](/uftpd.html)
recently I discovered that libuev has been missing from the tarball
since the release of the TFTP support.  Fixed.

Another great piece of news is that
[Coverity](http://www.coverity.com/) accepted uftpd as an Open Source
project, I've been hard at work fixing nasty bugs uncovered by the
[Coverity Scan](https://scan.coverity.com/).  Great stuff! :)

