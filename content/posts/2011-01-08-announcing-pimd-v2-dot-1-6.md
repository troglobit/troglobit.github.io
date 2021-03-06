---
categories:
- multicast
- opensource
comments: true
date: 2011-01-08T23:40:22Z
title: Announcing pimd v2.1.6
url: /2011/01/08/announcing-pimd-v2-dot-1-6/
aliases: /blog/2011/01/08/announcing-pimd-v2-dot-1-6/
---

This is a security release of [pimd][1], v2.1.6.  The following fixes
are included:

* [CVE-2011-0007][CVE]: *"Insecure file creation in /var/tmp"*
* Build error on GNU/kFreeBSD

Thanks to [Vincent Bernat][bernat] this time for the CVE fix!

As always, check the [homepage][1], the [ChangeLog][2] and the
[GIT log][3] for more details.

[1]: /pimd.html
[2]: https://github.com/troglobit/pimd/blob/master/ChangeLog
[3]: https://github.com/troglobit/pimd/commits/2.1.6
[CVE]: https://security-tracker.debian.org/tracker/CVE-2011-0007
[bernat]: http://www.luffy.cx/
