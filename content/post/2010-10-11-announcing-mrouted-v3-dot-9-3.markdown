---
categories:
- multicast
- opensource
comments: true
date: 2010-10-11T23:44:28Z
title: Announcing mrouted v3.9.3
url: /2010/10/11/announcing-mrouted-v3-dot-9-3/
aliases: /blog/2010/10/11/announcing-mrouted-v3-dot-9-3/
---

Here is another bug-fix release of [mrouted](/mrouted.html).  This time
with a couple of really nasty bugs fixed.  A big thanks to Dan Kruchinin
for tracking down a `NULL` pointer dereference in the conf file parser
and an overzealous check of netmask that made mrouted useless over
tunnel interfaces or point-to-point links (OpenVPN, PPP, L2TP and PPTP).
See the [ChangeLog][1] or the [GIT log][2] for details.

[1]: https://github.com/troglobit/mrouted/blob/master/ChangeLog
[2]: http://git.troglobit.com/mrouted.git/
