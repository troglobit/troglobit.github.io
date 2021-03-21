---
name: merecat
title: "Merecat httpd"
date: 2019-07-19 12:35:00 +02:00
aliases: /merecat.html
---
[Merecat][] started out as a pun at [Mongoose][], but is now useful for
actual web serving purposes.  It is however not a real [Meerkat][],
merely yet another copycat, forked from the great [thttpd][] created by
[Jef&nbsp;Poskanzer](http://www.acme.com).

Merecat expands on the features originally offered by thttpd, but still
has a limited feature set:

- Virtual hosts
- Basic `.htpassd` and `.htaccess` support
- URL-traffic-based throttling
- CGI/1.1
- HTTP/1.1 Keep-alive
- Built-in gzip deflate using zlib
- HTTPS support using OpenSSL/LibreSSL, works with [Let's Encrypt][]!
- Dual server support, both HTTP/HTTPS from one process
- HTTP redirect, to gently redirect a HTTP server to HTTPS
- Native PHP support, using `php-cgi` if enabled in `merecat.conf`

The first landing page after installation can be seen here:

:point_right: <https://merecat.troglobit.com>

<img class="center noborder" src="/images/peeking.gif" />

The resulting footprint (~140 kiB) makes it quick and suitable for small
and embedded systems!

Merecat is available as free/open source software under the simplified
2-clause [BSD license][license].  For more information, see the manual
page `merecat(8)`, or the [FAQ][].

Issue tracker and GIT repository available at GitHub:

   * [Repository](http://github.com/troglobit/merecat)
   * [Issue Tracker](http://github.com/troglobit/merecat/issues)
   * [ChangeLog](https://github.com/troglobit/merecat/blob/master/ChangeLog.md)
   * [merecat-2.32.tar.xz](ftp://ftp.troglobit.com/merecat/merecat-2.32.tar.xz),
     [MD5](ftp://ftp.troglobit.com/merecat/merecat-2.32.tar.xz.md5)


Origin & References
-------------------

Merecat is a stiched up fork of [sthttpd][] with lots of lost patches
found lying around the web.  The sthttpd project in turn is a fork from
the original [thttpd][] -- the tiny/turbo/throttling HTTP server.

* [thttpd][] was created by Jef Poskanzer <mailto:jef@mail.acme.com>
* [sthttpd][] was spawned by Anthony G. Basile <mailto:blueness@gentoo.org>
* [Merecat][] is maintained by Joachim Nilsson <mailto:troglobit@gmail.com>

[Merecat]:       https://merecat.troglobit.com
[Meerkat]:       https://en.wikipedia.org/wiki/Meerkat
[license]:       https://github.com/troglobit/merecat/blob/master/LICENSE
[Mongoose]:      https://github.com/cesanta/mongoose
[Let's Encrypt]: https://letsencrypt.org/
[FAQ]:           http://halplant.com:2001/server/thttpd_FAQ.html
[thttpd]:        http://www.acme.com/software/thttpd/
[sthttpd]:       https://github.com/blueness/sthttpd/
[License]:       https://en.wikipedia.org/wiki/BSD_licenses
