---
name: libeditline
title: "Minix Editline"
date: 2017-09-07 01:32:00 +02:00
aliases: /editline.html
---

This is a line editing library for UNIX.  It can be linked into almost
any program to provide command line editing and history.  It is call
compatible with the FSF readline library, but is a fraction of the size
(and offers fewer features).

<img class="center noborder" src="/images/peeking.gif" />

The small size (<30k), lack of dependencies (no ncurses needed!) and the
free license should make this library interesting to many embedded
developers seeking a replacement for the [GNU readline][] library.

Configuration is made by supplying different options to the GNU
configure script.  In the [examples/][] directory you can find some
small code snippets used for testing.

Issue tracker and GIT repository available at GitHub:

   * [Repository](http://github.com/troglobit/editline)
   * [Issue Tracker](http://github.com/troglobit/editline/issues)
   * [ChangeLog](https://github.com/troglobit/editline/blob/master/ChangeLog.md)
   * [editline-1.15.3.tar.xz](ftp://ftp.troglobit.com/editline/editline-1.15.3.tar.xz),
     [MD5](ftp://ftp.troglobit.com/editline/editline-1.15.3.tar.xz.md5)

See also the [OpenHub page](https://www.openhub.net/p/editline), or
the old [Free(code) page](http://freecode.com/projects/minix-editline).

Origin & References
-------------------

The editline library was created by Simmule Turner and [Rich Salz][]
back in 1992.  At the time they chose to distribute the code under a
"[C News][]-like" copyright, see the file [LICENSE][] for details.
Today [Rich](https://github.com/richsalz/) disitributes the
[original sources](https://github.com/richsalz/editline) under the
[Apache 2.0 license](https://github.com/richsalz/editline/blob/master/LICENSE).
The version distributed here, however, continues to use the original
license.

This version of the editline library is the continuation of the
[Minix3][] sources, where it is called libedit.  Other known versions,
often based on the original [comp.sources.unix][] posting are:

* [Debian libeditline](http://packages.qa.debian.org/e/editline.html)
* [Heimdal](http://www.h5l.org/), in
  [heimdal/lib/editline/](http://github.com/heimdal/heimdal/tree/master/lib/editline/)
* [Festival](http://festvox.org/festival/) speech-tools, in
  speech-tools/siod/
* Steve Tell's [editline patches](http://www.cs.unc.edu/~tell/dist.html)

The most intersting patches and bug fixes from each fork have been
merged here.  Outstanding issues are listed in the [TODO][] file.


[Rich Salz]:    http://en.wikipedia.org/wiki/Rich_Salz
[C News]:       http://en.wikipedia.org/wiki/C_News
[TODO]:         https://github.com/troglobit/editline/blob/master/TODO
[LICENSE]:      https://github.com/troglobit/editline/blob/master/LICENSE
[GNU readline]: http://directory.fsf.org/project/readline/
[examples/]:    https://github.com/troglobit/editline/tree/master/examples
[Minix3]:       http://www.minix3.org/
[comp.sources.unix]: http://ftp.cs.toronto.edu/pub/white/pub/rc/editline.shar

<!--
  -- Local Variables:
  -- mode: markdown
  -- End:
