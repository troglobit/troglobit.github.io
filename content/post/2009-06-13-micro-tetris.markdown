---
categories: null
comments: true
date: 2009-06-13T00:04:30Z
title: Micro Tetris™
url: /2009/06/13/micro-tetris/
aliases: /blog/2009/06/13/micro-tetris/
---

I just published the [unobfuscated version][1] of the
[1989 IOCCC Best Game][2] entry, Tetris™.  See the original author's,
[John Tromp][3], home page for the game for details.  But suffice it to
say, this is an extremely bare bones version of the classic game and
very similar to the [BSD games'][4] version.

Actually, this similarity in board layout, key combinations and feel led
me to do some archaeological digging.  I dowloaded the classic BSD games
collection and, after having called [GNU Indent][5] on the obfuscated
code, I started noticing such extreme similarities that just could not
be coincidental.  The layout and constructs of the code are virtually
the same, so I can only conclude that they must share the same ancestor.
Maybe this code is that ancestor, considering that the BSD Tetris is
copyrighted in 1992 and this version stems from 1989, it is maybe not a
completly illogical conclusion.

See for yourself.

* [tetris.git][6]
* [BSD Games][4]

[1]: http://git.troglobit.com/tetris.git
[2]: http://www.ioccc.org/1989/tromp.hint
[3]: http://tromp.github.io/tetris.html
[4]: http://www.ibiblio.org/pub/linux/games/bsd-games-2.17.tar.gz
[5]: http://www.gnu.org/software/indent/
[6]: http://git.troglobit.com/tetris.git

