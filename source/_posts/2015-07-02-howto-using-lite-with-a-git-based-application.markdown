---
layout: post
title: "HowTo: Using -lite with a GIT-based application"
date: 2015-07-02 17:11:55 +0200
comments: true
categories: 
- HowTo
- opensource
- Git
- submodules
---

Years ago while looking for a fast [init][1] replacement for [work][2],
I found [Finit][3].  Originally written by [Claudio Matsuoka][4] to act
as a drop-in replacement for the [Asus EeePC][5] [fastinit][6], *"gaps
filled with frog DNA &hellip;"*

Until I found Finit I had always been in awe of those venturing into the
realm of [PID 1][7], however, learning from the simplicity of Claudio's
code I realized that although indistinguishable from magic, it was not
too hard to master.  My version of Finit is [available on GitHub][14].

The code is open sourced under the very liberal [MIT/X11 license][MIT],
and much of its frog DNA has proven very useful to me over the years.
This blog post is about how that frog DNA can help you fill gaps in your
projects &hellip;

<!-- more -->

Recently I broke out the most generic pieces from Finit into a separate
library, which I call [libite][8] (because it looks awesome linking to
it: `-lite` :) I complemented it with a few pieces of my own and some
from the [OpenBSD][9] project, most notably their famous string API:
[strlcpy(3)][10] and [strlcat(3)][10].  Also included is the very useful
*BSD linked list API [sys/queue.h][11], which is a much more up to date
version than GLIBC carries.  For example, the new `_SAFE` macros are
missing, which you want to use while traversing lists to delete/free
nodes.

To make use of `-lite` and its APIs you can either build it as a
separate library and install `lite.h` and `libite.so.1` to your system,
or add `libite` as a GIT submodule to your project and use only the
parts you need from the archive:

    git submodule add https://github.com/troglobit/libite.git

You then need to add `#include "libite/lite.h"` to the source and adapt
your Makefile slightly to call the `libite/Makefile` before linking your
application to the `libite.a` archive:

    all: $(EXEC) libite/libite.a
    
    libite/libite.a: Makefile
            @$(MAKE) -C libite
    
    $(EXEC): $(OBJS) libite/libite.a
            @gcc -o $@ $^

For an example of how this can look, see the [uftpd][12] project, which
uses both `-lite` and `-luev`.  The latter is my small event library,
[libuEv][13].  For help using `-lite` with the GNU configure and build
system, see [inadyn][15].

Libite builds in "silent mode" by default, use `make V=1` (like the
kernel) to get a more verbose output, usable for autobuilders etc.

[1]: https://en.wikipedia.org/wiki/Init
[2]: http://westermo.com/
[3]: http://helllabs.org/finit/
[4]: https://github.com/cmatsuoka
[5]: https://en.wikipedia.org/wiki/Asus_Eee_PC
[6]: http://wiki.eeeuser.com/boot_process:the_boot_process
[7]: http://0pointer.net/blog/
[8]: https://github.com/troglobit/libite
[9]: http://www.openbsd.org/
[10]: http://www.openbsd.org/cgi-bin/man.cgi?query=strlcpy
[11]: http://www.openbsd.org/cgi-bin/man.cgi/OpenBSD-current/man3/LIST_EMPTY.3
[12]: https://github.com/troglobit/uftpd
[13]: https://github.com/troglobit/libuev
[14]: https://github.com/troglobit/finit
[15]: https://github.com/troglobit/inadyn
[MIT]: http://opensource.org/licenses/MIT
