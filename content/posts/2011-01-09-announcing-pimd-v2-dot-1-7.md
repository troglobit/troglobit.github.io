---
categories:
- multicast
- opensource
comments: true
date: 2011-01-09T20:20:57Z
title: Announcing pimd v2.1.7
url: /2011/01/09/announcing-pimd-v2-dot-1-7/
aliases: /blog/2011/01/09/announcing-pimd-v2-dot-1-7/
---

This is a followup release to the [security fix][CVE] in [pimd][1],
v2.1.6.  The change to use `/var/lib/misc/`, instead of the insecure
`/var/tmp/`, has now been refactored into using the proper [FHS][FHS]
recommended `/var/run/pimd/` instead.

As always, check the [homepage][1], the [ChangeLog][2] and the
[GIT log][3] for more details.

[1]: /pimd.html
[2]: https://github.com/troglobit/pimd/blob/master/ChangeLog
[3]: https://github.com/troglobit/pimd/commits/2.1.7
[CVE]: https://security-tracker.debian.org/tracker/CVE-2011-0007
[FHS]: http://www.pathname.com/fhs/pub/fhs-2.3.html#VARRUNRUNTIMEVARIABLEDATA
