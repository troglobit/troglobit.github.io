---
categories:
- opensource
- ddns
comments: true
date: 2014-05-14T08:29:48Z
title: New release of the DDNS client Inadyn
url: /2014/05/14/new-release-of-the-ddns-client-inadyn/
---

Looking for a Dynamic DNS, DDNS, client?  Well you're in luck, the
[FLOSS][1] market space is flooded with dedicated clients and various
wget scripts.  So why bother with an old C implementation?  Well, this
is admittedly one of the old timers in the game and is likely packaged
for your GNU/Linux distribution of choice already.  It's tried and
tested with many DDNS service providers and even comes bundled in a
few embedded router distributions as well.

Today I've released [Inadyn](/inadyn.html) v1.99.7 as an XZ compressed
tarball.  The release announcement should be up on [free(code)][2]
now.  It's a small incremental release containing mainly code cleanup
and a few new features that you likely won't notice: support for
multiple cache files and DDNS providers as separate plugins, for easy
addition of new ones as well as maintenance of existing.  This further
separates the internal logic from the provider specific parts and also
helps reduce clutter to ease maintenance.

This is a small incremental release building up to the long awaited
v2.0, which will include the long-awaited HTTPS support and also a new
.conf file format.  The latter is not really welcomed with open arms
by users, but is a necessary step since the current parser is heavily
affected by bit rot and must be put down.  I will attempt to make the
transition as easy as possible, but you will need to change you .conf
files.

Maybe a preview of the HTTPS support will make it into a release that
still has the old .conf file format, but just maybe.

[1]: http://en.wikipedia.org/wiki/Alternative_terms_for_free_software "FLOSS"
[2]: https://freecode.com/projects/inadyn
