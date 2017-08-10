---
layout: page
name: Inadyn
title: "Small and Simple DDNS Client"
sharing: true
footer: true
date: 2017-08-10 09:42
comments: false
---

Inadyn is a small and simple Dynamic DNS, [DDNS][1], client with HTTPS
support.  It is commonly available in many GNU/Linux distributions, used
in off-the-shelf routers and Internet gateways to automate the task of
keeping your DNS record up to date with any IP address changes from your
[ISP][2].  It can also be used in installations with redundant (backup)
connections to the Internet.

**Note:** The `.conf` file format syntax changed in v2.0!


Supported Providers
-------------------

The following is a curated list of *some* of the natively supported DDNS
providers.  Other providers, e.g. <http://twoDNS.de>, can usually be
supported using the custom provider support.  For the full details, see
the [README][], or `inadyn.conf(5)` found in the tarball.

* <https://freedns.afraid.org>
* <https://nsupdate.info>
* <https://duckdns.org>
* <https://freemyip.com>
* <https://www.loopia.com>
* <https://www.dyndns.org>, <https://dyn.com>
* <https://www.zoneedit.com>
* <https://www.easydns.com>
* <https://www.dnsomatic.com>
* <https://www.tunnelbroker.net>
* <https://dns.he.net>
* <https://www.changeip.com>
* <https://www.sitelutions.com>
* <https://www.dnsexit.com>
* <https://www.namecheap.com>
* <https://domains.google>
* <https://www.ovh.com>
* <https://www.dtdns.com>
* <https://www.duiadns.net>
* <https://ddnss.de>
* <https://dynv6.com>
* <https://spdyn.de>
* <https://www.strato.com>
* <https://www.cloudxns.net>
* <https://www.dnspod.cn>

Some of these services are free of charge for non-commercial use, others
take a small fee, but also provide more domains to choose from.


Download
--------

Issue tracker and GIT repository available at GitHub:

* [README](https://github.com/troglobit/inadyn/blob/master/README.md)
* [CHANGELOG](https://github.com/troglobit/inadyn/blob/master/CHANGELOG.md)
* [Repository](http://github.com/troglobit/inadyn)
* [Issue Tracker](http://github.com/troglobit/inadyn/issues)
* [inadyn-2.2.tar.xz](ftp://ftp.troglobit.com/inadyn/inadyn-2.2.tar.xz),
  [MD5](ftp://ftp.troglobit.com/inadyn/inadyn-2.2.tar.xz.md5)
  [GPG Sign](ftp://ftp.troglobit.com/inadyn/inadyn-2.2.xz.asc)

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
