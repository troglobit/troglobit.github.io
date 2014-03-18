---
layout: page
title: "Minix Editline"
sharing: true
footer: true
date: 2013-07-07 01:41
comments: false
---

This is a line editing library.  It can be linked into almost any
program to provide command-line editing and history.  It is
call-compatible with the FSF readline library, but is a fraction of
the size (and offers fewer features).

{% img center /images/peeking.gif 468 52 %}

The editline library was created by Simmule Turner and
[Rich Salz](http://en.wikipedia.org/wiki/Rich_Salz) back in 1992.  At
the time they chose to distribute the code under a
"[C News](http://en.wikipedia.org/wiki/C_News)-like"
copyright, see the file
[LICENSE](https://github.com/troglobit/editline/blob/master/LICENSE)
for details.

The small size (<30k), lack of dependencies (no ncurses needed!) and
the free license should make this library interesting to many embedded
developers seeking a replacement for the
[GNU readline](http://directory.fsf.org/project/readline/) library.

Configuration is made by supplying different options to the GNU
configure script.  In the
[examples/](https://github.com/troglobit/editline/tree/master/examples)
directory you can find some small code snippets used for testing.

This version of the editline library is a fork off of the
[Minix3](http://www.minix3.org/) sources, where it is called
libedit. Other known versions, often based off of the original
`comp.sources.unix` posting are:

* [Debian libeditline](http://packages.qa.debian.org/e/editline.html)
* [Heimdal](http://www.h5l.org/), in
  [heimdal/lib/editline/](http://github.com/heimdal/heimdal/tree/master/lib/editline/)
* [Festival](http://festvox.org/festival/) speech-tools, in
  speech-tools/siod/
* Steve Tell's [editline patches](http://www.cs.unc.edu/~tell/dist.html)

The most intersting patches and bug fixes from each fork have been
merged here.  Outstanding issues are listed in the
[TODO](https://github.com/troglobit/editline/blob/master/TODO) file.

Issue tracker and GIT repository available at GitHub:

   * [Repository](http://github.com/troglobit/editline)
   * [Issue Tracker](http://github.com/troglobit/editline/issues)
   * [editline-1.14.1.tar.xz](ftp://troglobit.com/editline/editline-1.14.1.tar.xz),
     [MD5](ftp://troglobit.com/editline/editline-1.14.1.tar.xz.md5)

See also the [Free(code) page](http://freecode.com/projects/minix-editline).


