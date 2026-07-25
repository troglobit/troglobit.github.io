---
name: mdnsd
title: "Embeddable Multicast DNS daemon"
date: 2026-07-25 18:30:00 +02:00
lastmod: 2026-07-25 18:30:00 +02:00
aliases: [ /mdnsd.html, /mdnsd ]
image: /images/mdnsd-card.png
description: "mdnsd is a small Multicast DNS and DNS-SD (service discovery) responder and library for advertising and discovering hosts and services on the local link."
tags: [ mdnsd, mdns, dns-sd, opensource ]
---

`mdnsd` is a small Multicast DNS and [DNS-SD][rfc6763] (service discovery)
responder and library for advertising hosts and services on the local
link.  It descends from [Jeremie Miller's][jeremie] original mDNS/mDNS-SD
daemon and is a lightweight alternative to [avahi][], small enough to
embed in firmware.

<!--more-->

Features
--------

- Multicast DNS responder with [RFC 6763][rfc6763] DNS-SD service discovery
- Dual-stack: answers over IPv4 (`224.0.0.251`) and IPv6 (`ff02::fb`),
  with A and AAAA records for the host and its services
- Service `PTR` records point at the instance, so they show up in
  `avahi-browse`, `mdns-scan`, and friends.  Replies bundle the matching
  `SRV`, `TXT`, and address records in the additional section, so clients
  skip the follow-up queries; key-only `TXT` flags work too
- Multiple addresses per interface, with all of a host's services under
  one host name
- Tracks interface and address changes in real time over netlink, no
  poll cycle -- start it before the interface even exists
- Ships as three parts: an embeddable C library (`libmdnsd`), the `mdnsd`
  daemon, and the `mquery` scan/debug tool
- Runs on GNU/Linux and the BSDs, needs no special privileges, and can
  drop to an unprivileged user

<a id="services"></a>

Advertising Services
--------------------

By default `mdnsd` reads service definitions from `/etc/mdns.d/*`.  The
file format is free-form; `type` and `port` are required, the rest is
optional.  See **[mdnsd.service(5)][man-service]** for the details.  An
HTTP service, for example:

    # /etc/mdns.d/http.service
    name Troglobit HTTP Server
    type _http._tcp
    port 80
    txt  server=merecat
    txt  version=2.31
    cname home.local

You need at least one service record for `mdnsd` to answer browse queries
from tools like `mdns-scan`.

<a id="browsing"></a>

Browsing with mquery
--------------------

The bundled `mquery` tool discovers and resolves services on the local
link, a bit like `avahi-browse`.  Give it a service type or a host name,
the `.local.` domain is implied:

    mquery                 # browse all service types on the link
    mquery _http._tcp      # list instances of a service type
    mquery -t 33 NAME      # resolve an instance's host and port (SRV)
    mquery -D              # scan and resolve into a device table

See **[mquery(1)][man-mquery]** for the full list of record types and
options.

<a id="local"></a>

Resolving .local Names
----------------------

`mdnsd` answers queries for the names it advertises; it does not make the
system resolver mDNS-aware.  To let programs on the host resolve `.local`
names (`ping foo.local`, `getaddrinfo()`), install `libnss-mdns` and add
it to `/etc/nsswitch.conf`:

    hosts: files mdns4_minimal [NOTFOUND=return] dns mdns4

The two halves are complementary: `mdnsd` answers for the names it
advertises, while `libnss-mdns` resolves everyone else's.

> **Note:** run either `mdnsd` or `avahi-daemon`, not both.  Two
> responders on the same link would answer over each other.

<a id="get"></a>

Get It
------

> [Latest release][releases] available on [GitHub][repo]

* [Repository][repo]
* [Download release tarballs][releases]
* [Release notes / ChangeLog][changelog]
* [Issue Tracker][issues]
* [API.md][api] -- using the `libmdnsd` library
* Man pages: [mdnsd.8][man-mdnsd], [mdnsd.service.5][man-service],
  [mquery.1][man-mquery]

Users who clone from GitHub must run `./autogen.sh` first; released
tarballs already ship a `configure` script.  `mdnsd` is licensed under
the 3-clause [BSD license][license].

<a id="origin"></a>

Origin & References
-------------------

This mDNS-SD implementation was developed by [Jeremie Miller][jeremie] in
2003, originally [announced on the rendezvous-dev][announced] mailing list
under the name *mhttp*.  It has many forks and has been used by a host of
other applications over the years.

The GitHub project is an effort by [Joachim Wiberg][troglobit] to clean up
the original code base, develop it further, and maintain it for the long
haul under the name `mdnsd`.

[jeremie]:     https://github.com/quartzjer
[troglobit]:   https://github.com/troglobit
[avahi]:       https://www.avahi.org/
[rfc6763]:     https://www.rfc-editor.org/rfc/rfc6763
[repo]:        https://github.com/troglobit/mdnsd
[releases]:    https://github.com/troglobit/mdnsd/releases
[changelog]:   https://github.com/troglobit/mdnsd/blob/master/ChangeLog.md
[issues]:      https://github.com/troglobit/mdnsd/issues
[api]:         https://github.com/troglobit/mdnsd/blob/master/API.md
[license]:     https://github.com/troglobit/mdnsd/blob/master/LICENSE
[announced]:   https://web.archive.org/web/20140115142008/http://lists.apple.com/archives/rendezvous-dev/2003/Feb/msg00062.html
[man-mdnsd]:   https://man.troglobit.com/man8/mdnsd.8.html
[man-service]: https://man.troglobit.com/man5/mdnsd.service.5.html
[man-mquery]:  https://man.troglobit.com/man1/mquery.1.html
