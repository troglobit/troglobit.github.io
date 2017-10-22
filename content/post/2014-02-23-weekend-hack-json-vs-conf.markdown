---
categories: null
comments: true
date: 2014-02-23T00:00:00Z
title: 'Weekend Hack: JSON vs CONF'
url: /2014/02/23/weekend-hack-json-vs-conf/
---

It was time.  I had been putting it off for far too long -- learning
about JSON and deciding on a new .conf file format for
[Inadyn](/inadyn.shtml).  So this weekend I sat myself down to read up
on [JSON](http://en.wikipedia.org/wiki/JSON) and the multitude of
parser libraries for both JSON and traditional .conf file parsers.  I
was looking for a human readable file format that a user could easily
and reliably edit by themselves without it being too error prone or
sensitive to mistyping.

{% gist 9177645 %}

I quickly narrowed down my scope of investigation to
[Jansson](http://www.digip.org/jansson/) for JSON and and
[Confuse](http://www.nongnu.org/confuse/) for "standard" UNIX .conf
files.  Both have very liberal licenses, extensive test suites, very
good documentation and are extremely well written!  In fact, I will
probably use them as prime examples of well maintained Open Source
projects in the future :-)

The resulting code is in my
[toolbox](https://github.com/troglobit/toolbox/tree/master/conf) on
GitHub.

{% gist 9177620 %}

In my opinion the .conf file is a lot easier to read, write and edit
by Joe User, so that's what I'll be using for many of my own projects,
starting with [Inadyn](/inadyn.shtml)