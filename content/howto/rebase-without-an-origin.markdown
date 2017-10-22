---
name:  "GIT rebase w/o origin"
title: "HowTo Rebase without an origin"
date: 2014-08-27 10:00:13 +0200
aliases:
    - /howto-rebase-without-an-origin.html
    - /blog/2014/08/27/howto-rebase-without-an-origin/index.html
categories: [ "git", "howto" ]
---

OK, so you've just been handed the task to integrate a big piece of
corporate software and it's nowhere near as done and ready as project
management thinks.  Of course you've already started chugging away at
it, fixing bugs, refactoring code and wrapping it in neat containers
to keep the changeset against the base SW small -- you already know
you'll get another drop of the same stinking pieace of code in another
six months, so you keep the changes small and track them in GIT with
neatly formatted commit messages.

About ten commits in it dawns on you that you have several commits
that would need to be squashed and some commit messages that needs to
be edited ... you start typing in `git rebase -i origin/...` wait, you
haven't pushed anything yet.  So how do you rebase something that's
not been pushed yet?

Well:

    git rebase --interactive --root master

Tip courtesy of my friend and collegue [wkz](https://github.com/wkz)

