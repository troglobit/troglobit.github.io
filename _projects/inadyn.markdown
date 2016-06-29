---
layout: page
name: Inadyn
title: "Small and Simple DDNS Client"
sharing: true
footer: true
date: 2015-09-09 21:29 +0100
comments: false
---

<a href="https://github.com/troglobit/inadyn"><img style="position: absolute; top: 0; right: 0; border: none; box-shadow: none;" src="https://camo.githubusercontent.com/365986a132ccd6a44c23a9169022c0b5c890c387/68747470733a2f2f73332e616d617a6f6e6177732e636f6d2f6769746875622f726962626f6e732f666f726b6d655f72696768745f7265645f6161303030302e706e67" alt="Fork me on GitHub" data-canonical-src="https://s3.amazonaws.com/github/ribbons/forkme_right_red_aa0000.png"></a>

<img class="right" src="/images/dyndns-multiple-sites.png" style="width: 500px; height: 300px;">

Inadyn is a small and simple [DDNS][1] client with HTTPS support, both
GnuTLS and OpenSSL are supported.  Inadyn is commonly available in many
GNU/Linux distributions, used in off-the-shelf routers and Internet
gateways to automate the task of keeping your DNS record up to date with
any IP address changes from your [ISP][2].  It can also be used in
installations with redundant (backup) connections to the Internet.


Supported Providers
-------------------

The following DDNS providers are supported natively, other providers,
like <http://twoDNS.de> for instance, can be supported using the generic
DDNS plugin.  See the [README][], or `inadyn.conf(5)` found in the
tarball, for configuration examples.

* <http://www.dyndns.org>
* <http://freedns.afraid.org>
* <http://www.zoneedit.com>
* <http://www.no-ip.com>
* <http://www.easydns.com>
* <http://www.tzo.com>
* <http://www.3322.org>
* <http://www.dnsomatic.com>
* <http://www.tunnelbroker.net>
* <http://dns.he.net/>
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

Some of these services are free of charge for non-commercial use, others
take a small fee, but also provide more domains to choose from.

Inadyn v1.99.8 and later support HTTPS (v1.99.11 and later also support
SNI), for DDNS providers that support this (you must check this
yourself).  Tested are DynDNS, FreeDNS, nsupdate.info, and Loopia.

Using HTTPS is recommended since it protects your credentials from being
snooped and further reduces the risk of someone hijacking your account!


Download
--------

Issue tracker and GIT repository available at GitHub:

* [README](https://github.com/troglobit/inadyn/blob/master/README.md)
* [CHANGELOG](https://github.com/troglobit/inadyn/blob/master/CHANGELOG.md)
* [Repository](http://github.com/troglobit/inadyn)
* [Issue Tracker](http://github.com/troglobit/inadyn/issues)
* [inadyn-1.99.15.tar.xz](ftp://troglobit.com/inadyn/inadyn-1.99.15.tar.xz),
  [MD5](ftp://troglobit.com/inadyn/inadyn-1.99.15.tar.xz.md5)
  [GPG Sign](ftp://troglobit.com/inadyn/inadyn-1.99.15.tar.xz.asc)

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
