---
layout: post
title: "Editline First Post"
date: 2009-06-14 20:44:45 +0200
comments: true
categories:
- Projects
- Editline
- Open Source
---

For a while now I've been maintaining a port of the [Minix][1] editline
library libedit.  Mainly for my own purposes, or rather on behalf of
[Westermo WeOS][2], where it is used in the CLI.  This library is the
same as the [Debian editline][3] package, even though the origin of that
package is [somewhat unclear][4] to me.

Today I decided to adjust the package name and bump the version number
to indicate that my port is the same, and now slightly more advanced,
than the Debian version.  To that end I've now changed the repository
name and prepared for merging with Debian. Getting this work accepted
back into Debian is a completely different issue.

Get the code as a [Bazaar][5] branch, or view its change log through the
[Loggerhead][6] web interface:

* bzr branch [http://bzr.vmlinux.org/editline][7]
* [Loggerhead web interface][7]
* [New homepage][8]

I now intend to do some further integration work, adapting the `debian/`
directory from editline-1.12-5 and smoothing out any remaining issues
before I release 1.13.0.

[1]: http://www.cise.ufl.edu/~cop4600/cgi-bin/lxr/http/source.cgi/lib/editline/
[2]: http://www.westermo.com
[3]: https://packages.qa.debian.org/e/editline.html
[4]: https://lists.debian.org/debian-devel/2000/05/msg00548.html
[5]: http://www.bazaar-vcs.org
[6]: https://launchpad.net/loggerhead
[7]: http://git.troglobit.com/editline.git
[8]: http://troglobit.com/editline.html
