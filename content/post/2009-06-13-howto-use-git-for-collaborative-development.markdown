---
categories:
- git
- bzr
- Bazaar
- HowTo
comments: true
date: 2009-06-13T00:36:07Z
title: HowTo use Git for Collaborative Development
url: /2009/06/13/howto-use-git-for-collaborative-development/
---

This is mainly some notes for myself so I don't forget.  Having worked
with [GNU Bazaar][1] before much of Git is still alien to me.

This HowTo is divided into two parts:

1. what happens on your laptop, and
2. what you must do on a remote server where you publish your changes

So, let's start stimple:

    laptop> mkdir projectX; cd projectX
    laptop> git init
    laptop> emacs file1.txt
    laptop> git add file1.txt
    laptop> git commit -m "Initial commit"

Thus far no suprises, right?  Now, some nasty git bits:

    laptop> emacs file1.txt
    laptop> git commit

Yep, doesn't work. You have to add -a to the command line for "all".

    laptop> git commit -a

OK, so next item.  How to publish this so others can see? Well, I have a
shell account on a remote server, so I naturally try:

    laptop> git push sftp://login@example.com/pub/git/projectX.git
    fatal: I don't handle protocol 'sftp'

Does not work. OK, next obvious choice:

    laptop> git push ssh://login@example.com/pub/git/projectX.git
    fatal: '/pub/git/projectX.git': unable to chdir or not a git archive
    fatal: The remote end hung up unexpectedly

Wow, not a clue as to how I should proceed.  After some Google-Foo I
found [this article][2] detail the steps for remote repos.  Very messy,
compared to Bazaar.

    laptop> ssh login@example.com
    server> cd /pub/git
    server> mkdir projectX.git; cd projectX.git
    server> git --bare init
    server> logout
    laptop> git remote add origin ssh://crash@vmlinux.org/pub/git/tetris.git
    laptop> git push
    No refs in common and none specified; doing nothing.
    Perhaps you should specify a branch such as 'master'.
    fatal: The remote end hung up unexpectedly
    error: failed to push some refs to 'ssh://login@example.com/pub/git/projectX.git'
    laptop> git push origin master
    Counting objects: 29, done.
    Compressing objects: 100% (26/26), done.
    Writing objects: 100% (29/29), 9.54 KiB, done.
    Total 29 (delta 10), reused 0 (delta 0)
    To ssh://login@example.com/pub/git/projectX.git
    * [new branch]      master -> master

Eventually I made it sing.  The first `git push` must use the references
to origin and master.  At least you don't have to care about that later,
after the first push command git remembers what you want.

Only one thing left, in the gitweb project overview projectX is listed
as having no description.  Of course, since I'm starting to get to know
git by now, I realised early this is probably not something that is
propagated through push — yep, I was right.  You have to change that on
the server.

    laptop> ssh login@example.com
    server> cd /pub/git/projectX.git
    server> echo "Secret Project-X use ROT13 to decode all source files" >description
    server> logout

All done! *phew*

[1]: http://bazaar-vcs.org/
[2]: http://toolmantim.com/articles/setting_up_a_new_remote_git_repository
