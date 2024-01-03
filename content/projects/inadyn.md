---
name: In-a-dyn
title: "Small and Simple DDNS Client"
date: 2020-02-20 16:29:00 +02:00
lastmod: 2024-01-03 10:55:00 +01:00
aliases: /inadyn.html
---

In-a-dyn is a small and simple Dynamic DNS, [DDNS][1], client with HTTPS
support.  It is commonly available in many GNU/Linux distributions, used
in off-the-shelf routers  and Internet gateways to automate  the task of
keeping your DNS record up to date with any IP address changes from your
[ISP][2].  It can also be  used in installations with redundant (backup)
connections to the Internet.

[Presentation here.](https://docs.google.com/presentation/d/14rT-LB5Ea_aHamlMFzVvT92ZU1DMWzVhav9OMOiVkpI/edit?usp=sharing)


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


Download
--------

Issue tracker and GIT repository available at GitHub:

* [README][]
* [ChangeLog](https://github.com/troglobit/inadyn/blob/master/ChangeLog.md)
* [Repository](https://github.com/troglobit/inadyn)
* [Issue Tracker](https://github.com/troglobit/inadyn/issues)


Origin & References
-------------------

This is  the continuation  of the  [original INADYN][origin]  project by
Narcis  Ilisei.  The  goal of  this  project is  to focus  on \*BSD  and
various embedded Linux  platforms, but *Cygwin is  also supported*.  All
sane patches addressing this target audience are welcome!


[1]: https://en.wikipedia.org/wiki/Dynamic_DNS
[2]: https://en.wikipedia.org/wiki/ISP
[README]: https://github.com/troglobit/inadyn/blob/master/README.md
[origin]: http://www.inatech.eu/inadyn/
