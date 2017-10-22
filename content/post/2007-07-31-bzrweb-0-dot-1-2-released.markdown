---
categories: null
comments: true
date: 2007-07-31T23:33:01Z
title: Bzrweb 0.1.2 Released
url: /2007/07/31/bzrweb-0-dot-1-2-released/
aliases: /blog/2007/07/31/bzrweb-0-dot-1-2-released/
---

The end of my own "summer of code" is here.  Seriously, this summer I
took it upon myself to clean up my act and move whole heartedly to the
[Bazaar][1] version control system for [my private projects][2].
Needless to say, I didn't get far.

Being quite lazy I haven't upgraded this server yet to the latest shiny
Debian 4.0.  This made it a bit hard to setup the new shiny
[Loggerhead web gui][3] (see it in action [here][4]) for Bazaar &mdash;
so instead I started fixing up the [old bzrweb][5] that I already had
setup a few years ago.

I'm new to Python, but, as I said, I'm quite lazy and Python seemed to
suit me perfectly.  Whipping up Bzrweb to support the bzr 0.15 I had on
my laptop (Ubuntu 7.04) was just enough to fiddle with in the warm
summer evenings of my vacation.  Now I find myself really enjoying
Python. :-)

So I decided to try and take up maintainership of Bzrweb.  I like it for
its simplicity and small list of dependencies (only Python 2.4).  Which
is something I will honor and continue to evolve.

Oh well, [here it is][6]:

* [bzrweb-0.1.2.tar.gz][7] ([md5sum][8])
* Release Notes: [[TXT]][9] [[PDF]][10]

No GPG signing yet, I've yet to figure out how GPG works, again.

Bug reports can be sent to me directly, see my address in the tarball or
[online][2].

[1]: http://www.bazaar-vcs.org/
[2]: https://web.archive.org/web/20090601005547/http://vmlinux.org/jocke/bzr/
[3]: https://web.archive.org/web/20090601005547/http://www.lag.net/loggerhead/
[4]: https://web.archive.org/web/20090601005547/http://codebrowse.launchpad.net/~mwhudson/loggerhead/production/changes
[5]: https://web.archive.org/web/20090601005547/http://mccormick.cx/dev/bzrweb/
[6]: ftp://vmlinux.org/pub/bzrweb/
[7]: ftp://vmlinux.org/pub/bzrweb/bzrweb-0.1.2.tar.gz
[8]: ftp://vmlinux.org/pub/bzrweb/bzrweb-0.1.2.tar.gz.md5sum
[9]: ftp://vmlinux.org/pub/bzrweb/ReleaseNotes-0.1.2.txt
[10]: ftp://vmlinux.org/pub/bzrweb/ReleaseNotes-0.1.2.pdf

