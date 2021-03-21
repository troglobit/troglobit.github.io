---
categories:
- projects
- icmp
comments: true
date: 2016-11-19T23:37:20Z
title: One ping only
url: /2016/11/19/one-ping-only/
aliases: /blog/2016/11/19/one-ping-only/
---

For some odd reason, today was the day when I woke up and continued
working on [libICMP][0].  It's now almost seven years since I first
adopted Tim Lawless' public domain [version][1], and today I picked
up where I left off and started refactoring and cleaning up.

<img src="/images/one-ping-only.jpg" style="width:50%;height:auto;" title="Sean Connery vs USS ICMP" class="center">

**Example:**

```c
#include "icmp/icmp.h"

int main(int argc, char *argv[])
{
    char *host = "localhost";
    struct libicmp *obj;
    
    if (argc >= 2)
            host = argv[1];

    if (!(obj = icmp_open(host, 0x1337, 0)))
            return 1;
    
    return icmp_ping(obj, 0, 0) == -1;
}
```

libICMP is nowhere near as fancy as [liboping][2] and is only slightly
smaller with a more liberal license ([ISC][3]).  The [first release][4]
is however available from GitHub.  Pull requests are as usual most
welcome! :smiley:

[0]: https://github.com/troglobit/libicmp/
[1]: https://packetstormsecurity.com/files/10728/libicmp.tar.gz.html
[2]: http://noping.cc/
[3]: https://en.wikipedia.org/wiki/ISC_license
[4]: https://github.com/troglobit/libicmp/releases/v1.0

<!--
  -- Local Variables:
  -- mode: markdown
  -- End:
  -->
