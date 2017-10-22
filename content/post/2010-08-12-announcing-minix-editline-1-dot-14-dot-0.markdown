---
categories:
- Minix
- Editline
comments: true
date: 2010-08-12T18:17:40Z
title: Announcing Minix editline 1.14.0
url: /2010/08/12/announcing-minix-editline-1-dot-14-dot-0/
aliases: /blog/2010/08/12/announcing-minix-editline-1-dot-14-dot-0/
---

It is with great pleasure I announce the next release of the
[editline][1] library by Simmule Turner and Rich Salz!  This is a
popular library, it exists in several forked versions.  This release
marks the end of a huge effort where archaeological methods have been
applied to recover fixes and improvements developed independently over
several decades by the following projects: [Heimdal][2],
[Festival speech-tools][3], [Debian][4], as well as patches by
Mr. [Steve Tell][5].

<img class="center noborder" src="/images/peeking.gif">

Noteworthy changes and additions:

* The symbols `rl_complete()` and `rl_list_possib()` are no longer
  possible to overload as function pointers.  Instead the FSF
  libreadline `rl_set_complete_func()` and `rl_set_list_possib_func()`
  API has been added for implementing custom completion handlers.
* 8-bit handling now work correctly on non-UTF8 terminals, including
  meta characters and key bindings.  This is actually a revert of an old
  broken Debian patch.
* The functions `el_bind_key()` and `el_bind_key_in_metamap()` have been
  added thanks to the efforts of Festival speech-tools hacker Mr. Alan
  W. Black.
* Support for capitalizing words (M-c), also from Mr. Alan W Black.
* Improved FSF libreadline compatibility and signal safety fixes by
  Mr. Steve Tell.
* The HUGE code cleanups are mostly courtesy of the Heimdal Project.
* Several more APIs for increased compatibility with FSF libreadline
  have also been added. See the file `include/editline.h` for details.

<img class="center noborder" src="/images/peeking.gif">

Online GIT repository and release tarball available at the usual places:

* [github:editline.git][6]  (Main GIT)
* [vmlinux:editline.git][7] (Backup)
* [editline-1.14.0.tar.bz2][8], [MD5][9] (FTP) 

Report bugs using the GitHub [issue tracker][10].  See also the
[Freshmeat page][11] for a more consistent updates.

[1]: /editline.html
[2]: http://www.h5l.org
[3]: http://festvox.org/festival/
[4]: http://packages.qa.debian.org/e/editline.html
[5]: http://www.cs.unc.edu/~tell/dist.html
[6]: http://github.com/troglobit/editline
[7]: http://git.troglobit.com/editline.git
[8]: ftp://ftp.troglobit.com/editline/editline-1.14.0.tar.bz2
[9]: ftp://ftp.troglobit.com/editline/editline-1.14.0.tar.bz2.md5
[10]: http://freshmeat.net/projects/minix-editline
[11]: http://github.com/troglobit/editline/issues
