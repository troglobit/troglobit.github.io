---
title: "Wireguard and DNS Timeout"
author: "Joachim Wiberg"
type: ""
date: 2020-07-17T11:04:09+02:00
subtitle: ""
image: ""
tags: []
---

For a while now my Wireguard VPN provider has been handing out a bad DNS
server.  So whenever I do a DNS lookup it takes five (5!) seconds timing
out, which is quite annoying.

This blog post is about how you can fix this with `openresolv` in Ubuntu.

<!--more-->

When you `apt install wireguard wireguard-tools` on Ubuntu 20.04, you
need a manager for `/etc/resolv.conf`.  There are two options here; the
traditional `resolvconf` package and the newer `openresolv`.  To fix my
problems I chose the latter.

With the system up and your `wg0` connection active (setting it up is
not the topic of this post), edit the file `/etc/resolvconf.conf`, yes
its named that even though we're using the `openresolv` package.  There
is lots of stuff already in it, but add the following:

```
$ sudo vim /etc/resolvconf.conf

# Slow/inactive DNS servers /Jocke
name_server_blacklist=2001:9b1:8826::53

# The edns0 option is from the original resolvconf
resolv_conf_options="edns0 timeout:1"

# Also from original resolvonf
append_search=localdomain
```

I've added my malfunctioning IPv6 DNS server to the blacklist.  This
means any provider wanting to add a DNS server to `/etc/resolv.conf`
will be filtered through that blacklist.

Finally, the `timeout:1` option (notice the double qoutes for multiple
options!).  I've set the timeout to 1, the default is 5 sec, in case
any other DNS server starts misbehaving.
