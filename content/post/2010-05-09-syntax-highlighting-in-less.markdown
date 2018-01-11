---
categories:
- unix
- shell
comments: true
date: 2010-05-09T19:53:19Z
title: Syntax highlighting in less
url: /2010/05/09/syntax-highlighting-in-less/
aliases: /blog/2010/05/09/syntax-highlighting-in-less/
---

<img style="float: right" src="/images/lessfilter.png">

Yes, it's certainly possible and source code becomes so much easier to
read.  Try it out by:

1. [downloading my .lessfilter][1]
2. save it as `~/.lessfilter` in your `$HOME`
3. Profit!

Ahem ...

Just try it out on a C source file :-)

    less -R myfile.c

[1]: https://github.com/troglobit/toolbox/blob/master/dot.lessfilter
