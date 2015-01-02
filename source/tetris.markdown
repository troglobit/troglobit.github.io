---
layout: page
title: "Micro Tetris&trade;"
sharing: true
footer: true
date: 2013-07-07 00:23
comments: false
---

<a href="https://github.com/troglobit/tetris"><img style="position: absolute; top: 0; right: 0; border: none; box-shadow: none;" src="https://camo.githubusercontent.com/365986a132ccd6a44c23a9169022c0b5c890c387/68747470733a2f2f73332e616d617a6f6e6177732e636f6d2f6769746875622f726962626f6e732f666f726b6d655f72696768745f7265645f6161303030302e706e67" alt="Fork me on GitHub" data-canonical-src="https://s3.amazonaws.com/github/ribbons/forkme_right_red_aa0000.png"></a>

[{% img right /images/TetrisConstruction.jpg 175 %}](http://erikjohanssonphoto.com/work/tetris/)

Micro Tetris &mdash; one of the smallest Tetris implementations in the
world!  Utilizing only ANSI escape sequences to draw the board, no
external library dependencys other than a standard C-library, like
[uClibc](http://www.uclibc.org/) or
[musl libc](http://www.musl-libc.org/).  Hence, it is very suitable
for small embedded devices in need of an easter egg ;-)

The game is based on an [entry](http://www.ioccc.org/1989/tromp.hint)
by [John Tromp](http://homepages.cwi.nl/~tromp/tetris.html) for the
1989 *International Obfuscated C Code Contest*.

Issue tracker and GIT repository available at GitHub:

   * [Repository](http://github.com/troglobit/tetris)
   * [Issue Tracker](http://github.com/troglobit/tetris/issues)
   * [tetris-1.2.1.tar.bz2](ftp://troglobit.com/tetris/tetris-1.2.1.tar.bz2),
     [MD5](ftp://troglobit.com/tetris/tetris-1.2.1.tar.bz2.md5)

See also the [Free(code) page](http://freecode.com/projects/micro-tetris).

{% img right /images/micro-tetris.png 136 %}

    j    Left
    k    Rotate
    l    Right
    SPC  Drop
    p    Pause
    q    Quit

