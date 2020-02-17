---
name: In-a-dyn
title: "Small and Simple DDNS Client"
date: 2020-02-17 12:12:00 +02:00
aliases: /inadyn.html
---

In-a-dyn is a small and simple Dynamic DNS, [DDNS][1], client with HTTPS
support.  It is commonly available in many GNU/Linux distributions, used
in off-the-shelf routers  and Internet gateways to automate  the task of
keeping your DNS record up to date with any IP address changes from your
[ISP][2].  It can also be  used in installations with redundant (backup)
connections to the Internet.

[Presentation here.](https://docs.google.com/presentation/d/14rT-LB5Ea_aHamlMFzVvT92ZU1DMWzVhav9OMOiVkpI/edit?usp=sharing)


Supported Providers
-------------------

The following is a curated list of *some* of the natively supported DDNS
providers.   Other providers,  e.g. <http://twoDNS.de>,  can usually  be
supported using the custom provider  support.  For the full details, see
the [README][], or `inadyn.conf(5)` found in the tarball.

   * <https://freedns.afraid.org>
   * <https://www.nsupdate.info>
   * <https://duckdns.org>
   * <https://freemyip.com>
   * <https://www.loopia.com>
   * <https://www.dyndns.org>, <https://dyn.com>
   * <https://www.noip.com>
   * <https://www.easydns.com>
   * <https://www.dnsomatic.com>
   * <https://dns.he.net>
   * <https://www.tunnelbroker.net>
   * <https://www.sitelutions.com>
   * <https://www.dnsexit.com>, parent of <https://www.zoneedit.com>
   * <https://www.changeip.com>
   * <https://www.dhis.org>
   * <https://www.namecheap.com>
   * <https://domains.google>
   * <https://www.ovh.com>
   * <https://giradns.com>
   * <https://www.duiadns.net>
   * <https://ddnss.de>
   * <https://dynv6.com>
   * <https://spdyn.de>
   * <https://www.cloudxns.net>
   * <https://www.pubyun.com>, formerly <http://www.3322.org>
   * <https://www.dnspod.cn>
   * <https://www.dynu.com>
   * <https://www.selfhost.de>
   * <https://connect.yandex.ru>
   * <https://www.cloudflare.com>

Some of these services are free of charge for non-commercial use, others
take a small fee, but also provide more domains to choose from.


Example
-------

The configuration file on most systems is in `/etc/inadyn.conf`:

    # In-A-Dyn v2.0 configuration file format
    period = 300
    
    # The FreeDNS username must be in lower case and
    # the password (max 16 chars) is case sensitive
    provider freedns.afraid.org {
        username = lower-case-username
        password = case-sensitive-pwd
        hostname = some.example.com
    }

In-a-dyn comes with a systemd unit file, so simply restart the service or
send `SIGHUP` to an already running inadyn to make it reload the `.conf`
file.  If you've built Inadyn yourself from source, the `.conf` file may
be located elsewhere.  See the `--prefix` argument to the `configure`
script, use `--help` or see the [README][] for details on building.

More examples in the `inadyn.conf(5)` man page and the [README][].

**Note:** The `.conf` file format syntax changed in v2.0!


Download
--------

Issue tracker and GIT repository available at GitHub:

* [README][]
* [ChangeLog](https://github.com/troglobit/inadyn/blob/master/ChangeLog.md)
* [Repository](https://github.com/troglobit/inadyn)
* [Issue Tracker](https://github.com/troglobit/inadyn/issues)
* [inadyn-2.5.tar.xz](ftp://ftp.troglobit.com/inadyn/inadyn-2.5.tar.xz),
  [MD5](ftp://ftp.troglobit.com/inadyn/inadyn-2.5.tar.xz.md5)
  [GPG Sign](ftp://ftp.troglobit.com/inadyn/inadyn-2.5.xz.asc)

See also the [OpenHub page](https://www.openhub.net/p/inadyn/), the
[Freshcode page](http://freshcode.club/projects/inadyn), or the dormant
[Free(code) page](http://freecode.com/projects/inadyn).


Origin & References
-------------------

This is  the continuation  of the  [original INADYN][origin]  project by
Narcis  Ilisei.  The  goal of  this  project is  to focus  on \*BSD  and
various embedded Linux  platforms, but *Cygwin is  also supported*.  All
sane patches addressing this target audience are welcome!

Relevant patches and features from the following forks have been merged:

* ["New" upstream](https://sourceforge.net/projects/inadyn/) by
  [Christoph Brill](http://www.egore911.de/)
* [Inadyn-advanced](https://sourceforge.net/projects/inadyn-advanced/)
* [Inadyn extended](https://github.com/vampik/inadyn) by
  [Vampik.ru](http://vampik.ru/), maintained by
  [Andrey Tikhomirov](https://github.com/vampik/inadyn)

The [inadyn-mt][] project is another fork from the original INADYN.  It
maintains support for Windows and adds many new features and fixes, not
just for Windows.

[1]: https://en.wikipedia.org/wiki/Dynamic_DNS
[2]: https://en.wikipedia.org/wiki/ISP
[README]: https://github.com/troglobit/inadyn/blob/master/README.md
[origin]: http://www.inatech.eu/inadyn/
[inadyn-mt]: https://sourceforge.net/projects/inadyn-mt/
