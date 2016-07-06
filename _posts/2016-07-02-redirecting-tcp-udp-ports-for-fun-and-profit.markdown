---
layout: post
title: "Redirecting Ports For Fun and Profit"
date: 2016-07-02 12:54:37 +0200
comments: true
categories:
- UNIX
---

Recently I needed a simple TCP/UDP port redirector and stumbled upon
[this Stackoverflow post][1].  As usual I wasn't first wanting to this
without using `iptables`.

There were several alternatives, but since my target was embedded with
limited amount of RAM and flash I wanted something really small.  So the
best fit turned out to be [redir][2], which unfortunately only could
handle TCP connections.  This is what led me to write [uredir][3] to
complement `redir`.  Eventually I ended up adoptiing `redir` as well,
which meant giving it a bit of a facelift and to give them both the same
look and feel.

Currently they are two separate applications, which in some use-cases
can be beneficial (small size), but I may in the future transplant the
UDP functionality of `uredir` into `redir`.  We'll see, right now though
I have several other [projects](/projects/) to attend to :-)

<!-- more -->

Examples
--------

To redirect port 80 to a webserver listening on loopback port 8080,
remember to use sudo when using priviliged ports:

    sudo redir :80 127.0.0.1:8080

To run redir from a process monitor like [Finit][4] or systemd, tell it
to not background itself and to only use the syslog for log messages:

    redir -n -s :80 127.0.0.1:8080

An `/etc/inetd.conf` line of the same looks very similar:

    http  stream  tcp  nowait  root  /usr/sbin/tcpd /usr/bin/redir -n -s -i 127.0.0.1:8080

When running multiple instances it can be useful to change how they
identify themselves.  The following starts an NNTP and a POP3 port
redirector, named accordingly.

    redir -I nntp www:119 netgate:119
    redir -I pop3 ftp:110 netgate:110

`uredir` works much in the same way, but also has a few UDP specific
features from the early days of the Internet:

    uredir 0.0.0.0:53 192.168.0.1:53
    uredir 0.0.0.0:7                   # Echo mode

An `/etc/inetd.conf` example:

    snmp    dgram   udp wait    root    /usr/sbin/tcpd /usr/bin/uredir -i 127.0.0.1:16161

See the `README` and man pages for each of the two commands for more
information.  Enjoy!

[1]: https://serverfault.com/questions/252150/port-forwarding-on-linux-without-iptables/
[2]: https://github.com/troglobit/redir
[3]: https://github.com/troglobit/uredir
[4]: https://github.com/troglobit/finit

<!--
  -- Local Variables:
  -- mode: markdown
  -- End:
  -->
