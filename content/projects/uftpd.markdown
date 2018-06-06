---
name: uftpd
title: "No nonsense FTP/TFTP server"
description: "FTP/TFTP server for Linux that just works™"
date: 2018-06-06 22:00:00 +0200
aliases: /uftpd.html
---

Tired of confusing configuration files and security features you don't need?

Try `uftpd(8)`!

* Supports both FTP and TFTP
* Has no confusing configuration file
* Listens to port `ftp/tcp` and `tftp/udp` found in `/etc/services`, or custom port(s)
* Serves files from the ftp user's $HOME, as specified in `/etc/passwd`, or custom path
* Can run from `inetd(8)` or as a standalone daemon
* Can be built and installed as a `.deb` file, with debconf support
* Can run as root, with chroot and privsep, or as a regular user
* Supports TFTP blocksize negotiation, [RFC 2348](http://tools.ietf.org/html/rfc2348)

Basically, it just works!

* [Repository](http://github.com/troglobit/uftpd)
* [README](https://github.com/troglobit/uftpd/blob/master/README.md)
* [TODO](https://github.com/troglobit/uftpd/blob/master/TODO.md)
* [Issue Tracker](http://github.com/troglobit/uftpd/issues)
* [uftpd-2.5.tar.xz](ftp://ftp.troglobit.com/uftpd/uftpd-2.5.tar.xz),
  [MD5](ftp://ftp.troglobit.com/uftpd/uftpd-2.5.tar.xz.md5),
* [ChangeLog](https://github.com/troglobit/uftpd/releases/tag/v2.5)

See also the [OpenHub page](https://www.openhub.net/p/uftpd/), the cool
[Fresh(code) page](http://freshcode.club/projects/uftpd), or the (sadly)
dormant [Free(code) page](http://freecode.com/projects/uftpd).

Read [my rant](/blog/2014/05/04/why-write-your-own-ftp-server/) if you
are curious to know why `uftpd` exists.  It provides a bit of background
and context :)

**Disclaimer:** `uftpd` supports both FTP and TFTP, but the latter is
  [not safe to use on the Internet](http://researchrepository.napier.ac.uk/8746/).
  FTP is safer and the `uftpd` implementation may even be secure.  Some
  of the most common security best practises have been employed, while
  others have been avoided in an effort to keep `uftpd` user friendly.
  If you want something really secure, you should probably try
  [vsftpd](https://security.appspot.com/vsftpd.html).

<!--
  -- Local Variables:
  -- mode: markdown
  -- End:
  -->
