---
layout: post
title: "Brief libev update"
date: 2009-04-05 23:53:47 +0200
comments: true
categories: 
---

I have now updated the [libev examples][1].  It took me a while, but
during that time I have been hard at work converting two of our network
daemons to use [libev][2].  As of today the upcoming [Westermo][3] WeOS
uses libev in both its rstpd and igmpd implementations.

Updated example code:

* [timer test][4]
* [message queue test][5]

Enjoy, and feel free to [contact me][6] if you have any questions or
comments on the code.  For libev specific help I can really recommend
their [mailing list][7]!

[1]: https://github.com/troglobit/toolbox/tree/master/libev-examples
[2]: http://software.schmorp.de/pkg/libev.html
[3]: http://www.westermo.com
[4]: https://github.com/troglobit/toolbox/blob/master/libev-examples/event-demo.c
[5]: https://github.com/troglobit/toolbox/blob/master/libev-examples/event-demo2.c
[6]: Joachim Nilsson <mailto:troglobit@gmail.com>
[7]: http://lists.schmorp.de/mailman/listinfo/libev
