---
title: "Minor Miracles"
date: 2019-06-26T19:07:29+02:00
subtitle: "Vacation Works"
tags: []
---

The last six months at work have been really stressful, so to finally
get a week off for Swedish Midsummer celebrations was really what the
doctor ordered!  I've managed to wind down and relax, spend time with my
kids and my family, and even put in some time on my personal software
projects!

Three years ago I forked [thttpd][] and began merging patches I'd found
scattered around the net.  I've also added a few features of my own, and
to avoid any confusion with the original (bug free!) project, I renamed
the resulting monster [Merecat](https://merecat.troglobit.com/) and it
has served <http://troglobit.com> and its subdomains since then.  Test
in production, yo!

Last year I added native support for HTTPS to Merecat, but it was still
not possible to serve both HTTP and HTTPS from the same httpd process,
so I began a slow refactor which I've now finalized! \o/

With a little help from [Let's Encrypt](https://letsencrypt.org/), I now
have <https://troglobit.com> up and running, and the HTTP site is also
running -- soon with automatic redirect to the HTTPS site.

Vacation works, it really does.

-----

[thttpd][] is a web server made by [Jef Poskanzer](http://acme.com/).
Jef is one of my big heroes, not only a great coder and over all nice
guy, he's also a great photographer!  Follow his [adventures on
Twitter](https://twitter.com/jef_poskanzer/)

[thttpd]: http://acme.com/software/thttpd/
