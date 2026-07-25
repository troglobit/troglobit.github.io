---
name: merecat
title: "Merecat httpd"
date: 2019-07-19 12:35:00 +02:00
lastmod: 2026-07-25 09:00:00 +0200
aliases: [ /merecat.html, /merecat ]
image: /images/merecat-card.png
description: "Merecat httpd is a tiny embedded web server forked from thttpd -- HTTPS, CGI, virtual hosts, and a reverse proxy in a ~160 kiB footprint."
tags: [ merecat, httpd, opensource ]
---

[Merecat][] started out as a pun at [Mongoose][], but is now useful for
actual web serving.  It is however not a real [Meerkat][], merely yet
another copycat, forked from the great [thttpd][] created by
[Jef&nbsp;Poskanzer](http://www.acme.com).

<!--more-->

Features
--------

Merecat expands on the features originally offered by thttpd, but keeps a
deliberately small feature set:

- Virtual hosts
- Basic `.htpasswd` and `.htaccess` support
- URL-traffic-based throttling
- CGI/1.1 and native PHP, using `php-cgi`, if enabled in `merecat.conf`
- HTTP/1.1 keep-alive
- Built-in gzip deflate using zlib
- HTTPS using OpenSSL/LibreSSL, works with [Let's Encrypt][]!
- Dual server support, both HTTP and HTTPS from one process
- HTTP redirect, to gently nudge an HTTP server over to HTTPS
- Reverse proxy (`proxy-pass`), to front local app servers the way nginx does

The resulting footprint (a ~160 kiB stripped binary) makes it quick and
suitable for small and embedded systems.  Merecat is free/open source
software under the simplified 2-clause [BSD license][license]; for more
details see the `merecat(8)` manual page or the [FAQ][].


Powered by Merecat
------------------

All of [troglobit.com][Merecat] runs on Merecat, including the page
you're reading.  The git browser over at [git.troglobit.com][git] is a
separate Go application that Merecat fronts with its reverse proxy
(`proxy-pass`) -- see [Can You Dig It?][digit] for that story.

The default landing page you get right after installation is live here:

**<https://merecat.troglobit.com>**

For more hands-on examples, see the HowTos:

- [Basic CGI in C][howto-cgi]
- [HTTPS with Let's Encrypt][howto-le]
- [Serving an ikiwiki][howto-wiki]
- [Set up cgit][howto-cgit]


Get It
------

> [Latest release][releases] available on [GitHub][repo]

* [Repository][repo]
* [Issue Tracker][issues]
* [ChangeLog][changelog]
* [Docker image][docker], to try it isolated from the rest of the system


Origin & References
-------------------

Merecat is a stitched-up fork of [sthttpd][] with lots of lost patches
found lying around the web.  The sthttpd project in turn is a fork of the
original [thttpd][] -- the tiny/turbo/throttling HTTP server.

* [thttpd][] was created by Jef Poskanzer <jef@mail.acme.com>
* [sthttpd][] was spawned by Anthony G. Basile <blueness@gentoo.org>
* [Merecat][] is maintained by Joachim Wiberg <troglobit@gmail.com>

[Merecat]:       https://merecat.troglobit.com
[Meerkat]:       https://en.wikipedia.org/wiki/Meerkat
[Mongoose]:      https://github.com/cesanta/mongoose
[Let's Encrypt]: https://letsencrypt.org/
[license]:       https://github.com/troglobit/merecat/blob/master/LICENSE
[FAQ]:           http://halplant.com:2001/server/thttpd_FAQ.html
[thttpd]:        http://www.acme.com/software/thttpd/
[sthttpd]:       https://github.com/blueness/sthttpd/
[repo]:          https://github.com/troglobit/merecat
[issues]:        https://github.com/troglobit/merecat/issues
[changelog]:     https://github.com/troglobit/merecat/blob/master/ChangeLog.md
[releases]:      https://github.com/troglobit/merecat/releases/latest
[docker]:        https://hub.docker.com/r/troglobit/merecat/
[git]:           https://git.troglobit.com
[digit]:         {{< ref "2026-07-12-digit-can-you-dig-it.md" >}}
[howto-cgi]:     {{< ref "merecat-basic-cig-in-c.md" >}}
[howto-le]:      {{< ref "merecat-and-lets-encrypt.md" >}}
[howto-wiki]:    {{< ref "merecat-and-ikiwiki.md" >}}
[howto-cgit]:    {{< ref "merecat-and-cgit.md" >}}
