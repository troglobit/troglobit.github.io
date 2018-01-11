---
categories:
- unix
- programming
comments: true
date: 2017-12-22T10:28:00Z
orig-date: 2015-11-14T11:45:49Z
title: Useful UNIX API:s
aliases:
    - /2015/11/14/useful-unix-api-s/
    - /blog/2015/11/14/useful-unix-api-s/
---

Had an interesting conversation with a buddy last night.  It started out
as a shift-reduce problem with Bison and ended up a ping-pong of useful
UNIX API:s.  We concluded that despite having worked professionally with
UNIX for over a decade, it is still very satisfying finding gems like
these.

Most people are completely unaware they exist and end up rolling their
own (buggy) implementations.  For instance, string manipulation and
various forms of linked lists.  Which is why I many years ago extracted
the *frog DNA* from [Finit](https://github.com/troglobit/finit/) to a
separate library called [libite](https://github.com/troglobit/libite/),
or -lite for short.  It imports the OpenBSD `strlcpy()` family of API:s,
up-to-date `queue.h` with the `_SAFE` iterators, and more.  Some people
like [libbsd](https://libbsd.freedesktop.org/wiki/) for this, but I've
found many of the ports incomplete and unsafe and prefer to stay closer
to the upstream *BSD versions.

**Update:** This post was originally written Nov 14, 2015.  It was a
Saturday and I remember being extremely inspired when I wrote it.  I've
continued adding to it over the years, and still do.  So, as of Jul 2,
2017 I'm bumping the modification date each time I add something new :-)

<!--more-->

SysV `search.h`
---------------

Mangage a [simple queue][sysvque]:

- [`insque()`](http://man7.org/linux/man-pages/man3/insque.3.html),
- [`remque()`](http://man7.org/linux/man-pages/man3/remque.3.html),

Manage [hash search table][sysvhash]:

- [`hsearch()`](http://man7.org/linux/man-pages/man3/hsearch.3.html)
- [`hcreate()`](http://man7.org/linux/man-pages/man3/hcreate.3.html)
- [`hdestroy()`](http://man7.org/linux/man-pages/man3/hdestroy.3.html)

Manage a [binary search tree][sysvtree]:

- [`tsearch()`](http://man7.org/linux/man-pages/man3/tsearch.3.html),
- [`tfind()`](http://man7.org/linux/man-pages/man3/tfind.3.html),
- [`tdelete()`](http://man7.org/linux/man-pages/man3/tdelete.3.html),
- [`twalk()`](http://man7.org/linux/man-pages/man3/twalk.3.html)
- [`tdestroy()`](http://man7.org/linux/man-pages/man3/tdestroy.3.html),

[Linear search][sysvlin] and update:

- [`lfind()`](http://man7.org/linux/man-pages/man3/lfind.3.html)
- [`lsearch()`](http://man7.org/linux/man-pages/man3/lsearch.3.html)


BSD `sys/queue.h`
-----------------

[This header][BSD] has lots of macros for handling various forms of
linked lists.  The version in GLIBC is a bit behind the BSD's, because
the latter also have `_safe()` versions of some macros to aid the user
in some tricky cases, e.g. when removing entries while iterating.

Several types of lists are supported:

- LIST: Doubly linked list
- SLIST: Single linked list
- STAILQ: Single linked tail queue
- SIMPLEQ: Simple queue
- TAILQ: Tail queue

Here's a few of them, this example for doubly linked lists:

- `LIST_INIT()`
- `LIST_EMPTY()`
- `LIST_FIRST()`
- `LIST_NEXT()`
- `LIST_REMOVE()`
- `LIST_FOREACH()`
- `LIST_INSERT_AFTER()`
- `LIST_INSERT_BEFORE()`
- `LIST_INSERT_HEAD()`

I wrote a [demo of the TAILQ API][DEMO] a couple of years ago.


Other Noteworthy API's
----------------------

Other (standard/POSIX) functions worthy of mentioning here are:

### `stdio.h`

- [`fmemopen()`](http://man7.org/linux/man-pages/man3/fmemopen.3.html)
- [`getdelim()`](http://man7.org/linux/man-pages/man3/getdelim.3.html)
- [`getline()`](http://man7.org/linux/man-pages/man3/getline.3.html)

### `stdlib.h`

- [`atexit()`](http://man7.org/linux/man-pages/man3/atexit.3.html)
- [`bsearch()`](http://man7.org/linux/man-pages/man3/bsearch.3.html)
- [`realpath()`](http://man7.org/linux/man-pages/man3/realpath.3.html)
- [`qsort()`](http://man7.org/linux/man-pages/man3/qsort.3.html)

### `glob.h`

- [`glob()`](http://man7.org/linux/man-pages/man3/glob.3.html)

### `fnmatch.h`

- [`fnmatch()`](http://man7.org/linux/man-pages/man3/fnmatch.3.html)

### `ftw.h`

- [`ftw()`](http://man7.org/linux/man-pages/man3/ftw.3.html)
- [`nftw()`](http://man7.org/linux/man-pages/man3/nftw.3.html)

### `unistd.h`

- [`access()`](http://man7.org/linux/man-pages/man2/access.2.html)
- [`alarm()`](http://man7.org/linux/man-pages/man2/alarm.2.html),
  [`pause()`](http://man7.org/linux/man-pages/man2/pause.2.html)
- [`crypt()`](http://man7.org/linux/man-pages/man3/crypt.3.html)
- [`daemon()`](http://man7.org/linux/man-pages/man3/daemon.3.html)
- [`pipe()`](http://man7.org/linux/man-pages/man2/pipe.2.html)
- [`profil()`](http://man7.org/linux/man-pages/man3/profil.3.html)

### `wordexp.h`

- [`wordexp()`](http://man7.org/linux/man-pages/man3/wordexp.3.html)

**Note:** API's specific to GNU or BSD are not included, but there are
  *many* more useful functions on your specific OS, in case you do not
need to write code that is portable across different UNIX platforms.
Examples can be `unshare()` and `clone()` on Linux and `pledge()` on
OpenBSD, all highly useful but also very specific.

[sysvque]:  http://pubs.opengroup.org/onlinepubs/009695399/functions/insque.html
[sysvlin]:  http://pubs.opengroup.org/onlinepubs/009695399/functions/lsearch.html
[sysvtree]: http://pubs.opengroup.org/onlinepubs/009695399/functions/tsearch.html
[sysvhash]: http://pubs.opengroup.org/onlinepubs/009695399/functions/hcreate.html
[BSD]:      https://www.freebsd.org/cgi/man.cgi?query=queue&sektion=3
[DEMO]:     https://github.com/troglobit/toolbox/blob/master/tailq-demo.c
