---
layout: post
title: "Learning About GIT"
date: 2009-04-10 16:23:42 +0100
comments: true
categories: 
- git
- svn
- subversion
- cvs
---

It has been a long time coming, but now I'm seriously starting to look
at [Git](http://git-scm.com/).  Git is the content tracker used by the
[Linux kernel](http://www.kernel.org) folks, initially developed by
Torvalds.

At [Westermo R&D](http://www.westermo.com) we use Subversion for our
daily operations and today I started migrating the software to Git.
Mainly just to get a comparison of performance, storage size and to
explore how we can use Git on top of svn to become more productive when
working in parallel in different teams.  We've tried this quite
extensively before, using Subversion branches and even though it's a
heck of a lot easier to use compared to CVS its strength is not in
merging.

The new
[merge tracking](http://subversion.tigris.org/merge-tracking/func-spec.html)
feature in Subversion 1.5 and features like
[detection of tree conflicts](http://subversion.tigris.org/svn_1.6_releasenotes.html#tree-conflicts)
in, the recently released, 1.6 helps.  However, working in small teams,
disconnected from a centralised server is still not possible with svn,
OK sure there is [svk](http://svk.bestpractical.com/), but honestly,
doesn't that make you think of CVS and bolting on features after the
fact, similar to [CVSNT](http://www.cvsnt.org/) or
[OpenCVS](http://opencvs.org/).  Intriguingly bizarre projects. :-)

The reasons for trying out Git are not only related to any short comings
of Subversion, Git has lot of strong points on its own.  See
[here](http://www.vijedi.net/2009/reasons-for-using-the-git-svn-bridge/)
for instance:

* SHA-1 hashes for each commit, makes it possible to easier detect history corruptions
* Local developer branches
* Fast branch switching
* Cherry picking commits from other branches, easily
* Stashing/Shelving, i.e. put aside things you're working on for later 

I've started collecting a set of links that seem to be useful:

* [Learning git-svn in five minutes](http://tsunanet.blogspot.com/2007/07/learning-git-svn-in-5min.html)
* [The git-svn(1) man page](http://www.kernel.org/pub/software/scm/git/docs/git-svn.html)
* [Git - SVN Crash Course](http://git.or.cz/course/svn.html)
* [An introduction to git-svn for Subversion users and deserters](http://utsl.gen.nz/talks/git-svn/intro.html)
* [How to use Git and SVN together](http://flavio.castelli.name/howto_use_git_with_svn)
* [A guided tour of emacs-git](http://tsgates.cafe24.com/git/git-emacs.html)
* [Work with Git from Emacs](http://xtalk.msk.su/~ott/en/writings/emacs-vcs/EmacsGit.html)
* [EmacsWiki:Git](http://www.emacswiki.org/emacs/Git)

I'll conclude this post with two YouTube entries, first is the, now
classic, presentation by
[Linus Torvalds on what Git isn't](http://www.youtube.com/watch?v=4XpnKHJAok8)
... and then
[Randal Schwartz on what Git is](http://www.youtube.com/watch?v=8dhZ9BXQgc4)
...
