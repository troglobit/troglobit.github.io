---
categories:
- git
- bzr
- bazaar
comments: true
date: 2009-06-12T19:50:27Z
title: More about Bzrweb and some about Git
url: /2009/06/12/more-about-bzrweb-and-some-about-git/
aliases: /blog/2009/06/12/more-about-bzrweb-and-some-about-git/
---

OK, I admit it.  I cannot decide what version control system (VCS) to
use.  I'm stuck between the speed and massive snow ball effect of
[Git][1] and the ease of use and emotional attachment I have to
[Bazaar][2].

I've been "maintaining" bzrweb for a while now, not doing a very good
job of it though.  It's lagging behind considerably to the bzr API.
After the upgrade of vmlinux.org to the latest Ubuntu server release
bzrweb actually didn't work at all.  If it hadn't been for
[the fixes][3] by [Rasmus Toftdahl Olesen][4] I would probably have
abandoned it entirely.  Thank you Rasmus!  Anyway, I've been gleaning at
other Bazaar web frontends for a while.  The only real option is
Loggerhead, but even though I consider myself a computer pro I just
cannot seem to wrap my head around how to set it up.  I just wanted to
setup a local browser for my user, without the need for root access.
Like bzrweb supports, after having abandoned that, seemingly radical
idea, I tried setting up a site wide shared installation ... no luck so
far. :-/

In comparison to Loggerhead I easily managed to setup a Git repository
browser, using a `/pub/git` structure, see [git.vmlinux.org][5].  The
gitweb package in Ubuntu was very easy to setup, the one only thing I
had problem with was the Apache VirtualHost setup.  Some sleep cured
that, but I should post the conf here, in case someone else experiences
trouble.

I'll have another go at setting up Loggerhead later, or perhaps try to
fixup bzrweb.  I'm currently leaning towards fixing up bzrweb.  Fix bugs
viewing tree files, refactor tarball export and a new project summary
page, are some of the most interesting things I can come up with.  If
only things could settle down at work for a while — oh well, vacation is
coming up. Soon, my precious, soon&nbsp;...

First, however, I will publish the micro vt100/ansi tetris version I
found last week.  It will be the second project where I use Git, the
first one was `cons`, which is a Xen xm wrapper for non privileged
users.

Some Git and Bazaar links:

* [Git for the Lazy][6] :-)
* [CVS, Subversion & Git Refcards][7]
* [Converting Bzr to Git][8]

[1]: http://git-scm.com/
[2]: http://bazaar-vcs.org/
[3]: https://code.launchpad.net/~halfdan/bzrweb
[4]: http://halfdans.net/
[5]: http://git.troglobit.com/
[6]: http://www.spheredev.org/wiki/Git_for_the_lazy
[7]: http://appletree.or.kr/quick_reference_cards/CVS-Subversion-Git/
[8]: http://www.fthieme.net/drupal6/node/77
