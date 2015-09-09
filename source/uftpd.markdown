---
layout: page
title: "uftpd -- the no nonsense (T)FTP server"
sharing: true
footer: true
date: 2015-07-23 02:07 +0100
comments: false
---

<a href="https://github.com/troglobit/uftpd"><img style="position: absolute; top: 0; right: 0; border: none; box-shadow: none;" src="https://camo.githubusercontent.com/365986a132ccd6a44c23a9169022c0b5c890c387/68747470733a2f2f73332e616d617a6f6e6177732e636f6d2f6769746875622f726962626f6e732f666f726b6d655f72696768745f7265645f6161303030302e706e67" alt="Fork me on GitHub" data-canonical-src="https://s3.amazonaws.com/github/ribbons/forkme_right_red_aa0000.png"></a>

Tired of confusing configuration files and security features you don't need?

* uftpd supports both FTP and TFTP,
* Has no confusing configuration file,
* Listens to port ftp/tcp and tftp/udp found in `/etc/services`
* Serves files from the ftp user's home directory, as specified in `/etc/passwd`
* Can run from inetd or as a standalone daemon
* Can run as root, with chroot and privsep, or as a regular user
* Supports TFTP blocksize negotiation, [RFC 2348](http://tools.ietf.org/html/rfc2348)

It just works!

**Disclaimer:** *uftpd was never made for the Internet, it may work
  and it may even be secure.  The developer has employed some of the
  most common techniques, but avoided others in an effort to keep
  uftpd user friendly.  Nevertheless, uftpd is intended for home
  users, developers of embedded systems (me), or other users inside an
  already secured local area network.  If you want something really
  secure, you should probably try
  [vsftpd](https://security.appspot.com/vsftpd.html)*

Want to know why this exists?  If you're really interested, read
[my rant](/blog/2014/05/04/why-write-your-own-ftp-server/) for
background and context :)

* [Repository](http://github.com/troglobit/uftpd)
* [README](https://github.com/troglobit/uftpd/blob/master/README.md)
* [TODO](https://github.com/troglobit/uftpd/blob/master/TODO.md)
* [Issue Tracker](http://github.com/troglobit/uftpd/issues)
* [uftpd-1.9.tar.xz](ftp://troglobit.com/uftpd/uftpd-1.9.tar.xz),
  [MD5](ftp://troglobit.com/uftpd/uftpd-1.9.tar.xz.md5)
* [uftpd_1.9-1_amd64.deb](ftp://troglobit.com/uftpd/uftpd_1.9-1_amd64.deb),
  [changes](ftp://troglobit.com/uftpd/uftpd_1.9-1_amd64.changes)

See also the [OpenHub page](https://www.openhub.net/p/uftpd/), or the
(sadly) dormant [Free(code) page](http://freecode.com/projects/uftpd).

<!--
  -- Local Variables:
  -- mode: markdown
  -- End:
  -->
