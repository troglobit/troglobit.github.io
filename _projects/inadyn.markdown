---
layout: page
name: Inadyn
title: "Small and Simple DDNS Client"
sharing: true
footer: true
date: 2015-09-09 21:29 +0100
comments: false
---

Inadyn is a small and simple [DDNS][1] client with HTTPS support.  It
is commonly available in many GNU/Linux distributions, used in
off-the-shelf routers and Internet gateways to automate the task of
keeping your DNS record up to date with any IP address changes from
your [ISP][2].  It can also be used in installations with redundant
(backup) connections to the Internet.

**Note:** The `.conf` file format syntax changed in v2.0!


Supported Providers
-------------------

The following is a list of *some* natively supported DDNS providers.
Other providers, e.g. <http://twoDNS.de>, can be supported using the
custom provider support.  See the [README][], or `inadyn.conf(5)`
found in the tarball, for configuration examples.

* <http://www.dyndns.org>
* <http://freedns.afraid.org>
* <http://www.zoneedit.com>
* <http://www.no-ip.com>
* <http://www.easydns.com>
* <http://www.tzo.com>
* <http://www.3322.org>
* <http://www.dnsomatic.com>
* <http://dns.he.net/>
* <http://www.tunnelbroker.net>
* <http://www.dynsip.org>
* <http://www.sitelutions.com>
* <http://www.dnsexit.com>
* <http://www.changeip.com>
* <http://www.zerigo.com>
* <http://www.dhis.org>
* <https://nsupdate.info>
* <http://duckdns.org>
* <https://www.loopia.com>
* <https://www.namecheap.com>
* <https://domains.google.com>
* <https://www.ovh.com>
* <https://www.dtdns.com>
* <http://giradns.com>
* <https://www.duiadns.net>
* <https://ddnss.de>
* <http://dynv6.com>
* <http://ipv4.dynv6.com>
* <https://spdyn.de>
* <https://www.strato.com>

Some of these services are free of charge for non-commercial use, others
take a small fee, but also provide more domains to choose from.


Download
--------

Issue tracker and GIT repository available at GitHub:

* [README](https://github.com/troglobit/inadyn/blob/master/README.md)
* [CHANGELOG](https://github.com/troglobit/inadyn/blob/master/CHANGELOG.md)
* [Repository](http://github.com/troglobit/inadyn)
* [Issue Tracker](http://github.com/troglobit/inadyn/issues)
* [inadyn-2.1.tar.xz](ftp://ftp.troglobit.com/inadyn/inadyn-2.1.tar.xz),
  [MD5](ftp://ftp.troglobit.com/inadyn/inadyn-2.1.tar.xz.md5)
  [GPG Sign](ftp://ftp.troglobit.com/inadyn/inadyn-2.1.xz.asc)

See also the [OpenHub page](https://www.openhub.net/p/inadyn/), the
[Freshcode page](http://freshcode.club/projects/inadyn), or the dormant
[Free(code) page](http://freecode.com/projects/inadyn).


Origin & References
-------------------

This is the continuation of the [original INADYN][origin] project by
Narcis Ilisei.  The goal of this project is to focus on \*BSD and
various embedded Linux platforms, but *Cygwin is also supported*.  All
sane patches addressing this target audience are welcome!

Included so far are relavant fixes and additions from the following
forks:

* ["New" upstream](https://sourceforge.net/projects/inadyn/) by
  [Christoph Brill](http://www.egore911.de/)
* [Inadyn-advanced](https://sourceforge.net/projects/inadyn-advanced/)
* [Inadyn extended](https://github.com/vampik/inadyn) by
  [Vampik.ru](http://vampik.ru/), maintained by
  [Andrey Tikhomirov](https://github.com/vampik/inadyn)

The [inadyn-mt][] project is another fork from the original INADYN.  It
maintains support for Windows and adds many new features and fixes, not
just for Windows.

[1]: http://en.wikipedia.org/wiki/Dynamic_DNS
[2]: http://en.wikipedia.org/wiki/ISP
[README]: https://github.com/troglobit/inadyn/blob/master/README.md
[origin]: http://www.inatech.eu/inadyn/
[inadyn-mt]: http://sourceforge.net/projects/inadyn-mt/

<!--
  -- Local Variables:
  -- mode: markdown
  -- End:
  -->
