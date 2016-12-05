---
layout: post
title: "Useful UNIX API:s"
date: 2015-11-14 11:45:49 +0100
comments: true
categories: 
---

Had an interesting conversation with a buddy last night.  It started out
as a shift-reduce problem with Bison and ended up a ping-pong of useful
UNIX API:s.  We concluded that despite having worked professionally with
UNIX for over a decade, it is still very satisfying finding gems like
these.

Most people are completely unaware they exist and end up rolling their
own (buggy) implementations.

<!-- more -->

SysV search.h
-------------

Mangage a [simple queue][sysvque]:

- `insque()`
- `remque()`

Manage [hash search table][sysvhash]:

- `hsearch()`
- `hcreate()`
- `hdestroy()`

Manage a [binary search tree][sysvtree]:

- `tsearch()`
- `tfind()`
- `tdelete()`
- `twalk()`
- `tdestroy()`

[Linear search][sysvlin] and update:

- `lfind()`
- `lsearch()`


BSD sys/queue.h
---------------

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

### stdio.h

- `fmemopen()`

### stdlib.h

- `atexit()`
- `bsearch()`
- `realpath()`
- `qsort()`

### glob.h

- `glob()`

### fnmatch.h

- `fnmatch()`

### ftw.h

- `ftw()`
- `nftw()`

### search.h

- `hsearch()`
- `lsearch()`
- `tsearch()`, `tfind()`, `twalk()`

### unistd.h

- `access()`
- `alarm()`, `pause()`
- `crypt()`
- `daemon()`
- `pipe()`
- `profil()`

**Note:** API's specific to GNU or *BSD are not included, but there
exist *many* more useful functions on your specific OS, in case you
do not need to write code that is portable across different UNIX
platforms.  Examples can be `unhare()` and `clone()` on Linux and
`pledge()` on OpenBSD, all highly useful but also very specific.

[sysvque]:  http://pubs.opengroup.org/onlinepubs/009695399/functions/insque.html
[sysvlin]:  http://pubs.opengroup.org/onlinepubs/009695399/functions/lsearch.html
[sysvtree]: http://pubs.opengroup.org/onlinepubs/009695399/functions/tsearch.html
[sysvhash]: http://pubs.opengroup.org/onlinepubs/009695399/functions/hcreate.html
[BSD]:      https://www.freebsd.org/cgi/man.cgi?query=queue&sektion=3
[DEMO]:     https://github.com/troglobit/toolbox/blob/master/tailq-demo.c
