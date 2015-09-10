---
layout: page
title: "Minix Editline"
sharing: true
footer: true
date: 2015-09-10 04:34
comments: false
---

<a href="https://github.com/troglobit/editline"><img style="position: absolute; top: 0; right: 0; border: none; box-shadow: none;" src="https://camo.githubusercontent.com/365986a132ccd6a44c23a9169022c0b5c890c387/68747470733a2f2f73332e616d617a6f6e6177732e636f6d2f6769746875622f726962626f6e732f666f726b6d655f72696768745f7265645f6161303030302e706e67" alt="Fork me on GitHub" data-canonical-src="https://s3.amazonaws.com/github/ribbons/forkme_right_red_aa0000.png"></a>

This is a line editing library for UNIX, with a focus on GNU/Linux.  It
can be linked into almost any program to provide command line editing
and history.  It is call compatible with the FSF readline library, but
is a fraction of the size (and offers fewer features).

{% img center /images/peeking.gif 468 52 %}

The editline library was created by Simmule Turner and [Rich Salz][]
back in 1992.  At the time they chose to distribute the code under a
"[C News][]-like" copyright, see the file [LICENSE][] for details.

The small size (<30k), lack of dependencies (no ncurses needed!) and the
free license should make this library interesting to many embedded
developers seeking a replacement for the [GNU readline][] library.

Configuration is made by supplying different options to the GNU
configure script.  In the [examples/][] directory you can find some
small code snippets used for testing.

This version of the editline library is a fork off of the [Minix3][]
sources, where it is called libedit. Other known versions, often based
off of the original `comp.sources.unix` posting are:

* [Debian libeditline](http://packages.qa.debian.org/e/editline.html)
* [Heimdal](http://www.h5l.org/), in
  [heimdal/lib/editline/](http://github.com/heimdal/heimdal/tree/master/lib/editline/)
* [Festival](http://festvox.org/festival/) speech-tools, in
  speech-tools/siod/
* Steve Tell's [editline patches](http://www.cs.unc.edu/~tell/dist.html)

The most intersting patches and bug fixes from each fork have been
merged here.  Outstanding issues are listed in the [TODO][] file.

Issue tracker and GIT repository available at GitHub:

   * [Repository](http://github.com/troglobit/editline)
   * [Issue Tracker](http://github.com/troglobit/editline/issues)
   * [editline-1.15.20tar.xz](ftp://troglobit.com/editline/editline-1.15.0.tar.xz),
     [MD5](ftp://troglobit.com/editline/editline-1.15.0.tar.xz.md5)

See also the [OpenHub page](https://www.openhub.net/p/editline), or
the old [Free(code) page](http://freecode.com/projects/minix-editline).

[Rich Salz]:    http://en.wikipedia.org/wiki/Rich_Salz
[C News]:       http://en.wikipedia.org/wiki/C_News
[TODO]:         https://github.com/troglobit/editline/blob/master/TODO
[LICENSE]:      https://github.com/troglobit/editline/blob/master/LICENSE
[GNU readline]: http://directory.fsf.org/project/readline/
[examples/]:    https://github.com/troglobit/editline/tree/master/examples
[Minix3]:       http://www.minix3.org/
