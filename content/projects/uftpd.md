---
name: uftpd
title: "No nonsense FTP/TFTP server"
date: 2019-07-29 20:31:00 +0200
lastmod: 2026-09-09 19:00:00 +02:00
aliases: [ /uftpd.html, /uftpd ]
image: /images/uftpd-card.png
description: "uftpd is a small FTP and TFTP server for the LAN.  No configuration file and no users to manage: it serves the ftp user's home directory, from inetd or as a standalone daemon."
tags: [ uftpd, ftp, tftp, opensource ]
---

`uftpd` is an FTP and TFTP server for the LAN.  There is no
configuration file and no users to manage.  It serves the `ftp` user's
home directory on the ports `/etc/services` gives it, and that is the
whole setup.

<!--more-->

Both protocols run in one daemon, over IPv4 and IPv6, from `inetd(8)` or
standalone.  Read-only unless you say otherwise.

Features
--------

- FTP and TFTP from a single daemon, dual-stack out of the box.  The FTP
  data channel works over IPv6 with `EPSV` and `EPRT`; the legacy `PASV`
  and `PORT` stay IPv4, as the protocol requires
- No configuration file.  Ports come from `/etc/services`, or from
  `-o ftp=PORT` and `-o tftp=PORT`
- Serves the `ftp` user's `$HOME` from `/etc/passwd`, or a path given on
  the command line
- Runs from `inetd(8)`, neatly tcpwrapped, or as a standalone daemon
- Read-only by default; `-o writable` allows uploads, over both FTP and
  TFTP
- As root it chroots and drops privileges; as a regular user it serves
  what that user can read
- TFTP block size negotiation, [RFC 2348][rfc2348], for transfers that
  are not stuck with 512 byte blocks
- `-o pasv_addr=ADDR` overrides the address handed to clients in passive
  mode, for the NAT setups that need it
- Builds as a `.deb` with debconf, which sets up `inetd.conf`, creates
  the `ftp` user, and prepares `/srv/ftp`

Running It
----------

From `inetd(8)`, which is what the `.deb` configures, the entries point
at `in.ftpd` and `in.tftpd`, both symlinks to `uftpd`:

    ftp   stream tcp nowait root /usr/sbin/tcpd in.ftpd
    tftp  dgram  udp wait   root /usr/sbin/tcpd in.tftpd

Standalone, serving a directory of your choosing:

    uftpd -n -l info /srv/tftp

`-n` keeps it in the foreground and `-l` sets the log level.  Everything
else has a default worth keeping.  See **[uftpd(8)][man-uftpd]** for the
rest.

Netbooting
----------

TFTP mostly exists to hand kernels and device trees to bootloaders, which
is where `uftpd` earns its keep.  Point U-Boot at it:

    setenv serverip 192.168.1.1
    tftpboot 0x68000000 vmlinuz

Bootloaders are strict clients, and they are why several corners of the
TFTP code have been sharpened over the years: option acknowledgements
without padding, retransmit on a duplicate ACK, and block numbers that
survive the 16-bit rollover on large images.

Get It
------

> [Latest release][releases] available on [GitHub][repo]

* [Repository][repo]
* [Download release tarballs][releases]
* [Release notes / ChangeLog][changelog]
* [Issue Tracker][issues]
* Man page: [uftpd.8][man-uftpd]

Users who clone from GitHub must run `./autogen.sh` first; released
tarballs already ship a `configure` script.  `uftpd` depends on
[libuEv][] and [libite][], and is licensed under the [ISC
license][license].

Security
--------

`uftpd` is meant for the LAN.  It is read-only by default, chroots and
drops privileges when started as root, and has no user database to get
wrong.

TFTP has no authentication at all and is [not safe to expose to the
Internet][napier].  FTP is the safer of the two, though anonymous by
design.  Some of the common hardening practices are in place and others
were left out to keep the thing usable, so if you need a server that is
secure before it is friendly, use [vsftpd][].

Origin & References
-------------------

I wrote `uftpd` because every FTP server I tried wanted a configuration
file, a user database, and an afternoon, when all I wanted was to move a
file across the desk.  The [longer version is here][rant].

[rfc2348]:   https://www.rfc-editor.org/rfc/rfc2348
[repo]:      https://github.com/troglobit/uftpd
[releases]:  https://github.com/troglobit/uftpd/releases
[changelog]: https://github.com/troglobit/uftpd/blob/master/ChangeLog.md
[issues]:    https://github.com/troglobit/uftpd/issues
[license]:   https://github.com/troglobit/uftpd/blob/master/LICENSE
[libuEv]:    https://github.com/troglobit/libuev
[libite]:    https://github.com/troglobit/libite
[man-uftpd]: https://man.troglobit.com/man8/uftpd.8.html
[napier]:    http://researchrepository.napier.ac.uk/8746/
[vsftpd]:    https://security.appspot.com/vsftpd.html
[rant]:      /2014/05/04/why-write-your-own-ftp-server/
