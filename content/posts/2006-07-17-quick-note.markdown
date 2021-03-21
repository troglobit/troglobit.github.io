---
categories:
- networking
- ubuntu
- debian
comments: true
date: 2006-07-17T21:10:45Z
title: Quick Note
url: /2006/07/17/quick-note/
aliases: /blog/2006/07/17/quick-note/
---

About that `/etc/network/interface` autoconfig thingy: it's almost done.
Works perfect ... at work (connected) and at home (wireless).  I need to
fine tune the tests, I think, to get the connected stuff to work at home
too.  The problem seems to be that the test to see if the machine is
connected at home is that the MAC it looks for to ID the office network
also is available from home, via the VPN.  I'll get back on the subject
as soon as I've done some more tests.
