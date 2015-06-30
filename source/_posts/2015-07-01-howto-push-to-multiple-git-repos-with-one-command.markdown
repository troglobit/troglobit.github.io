---
layout: post
title: "HowTo: Push to multiple GIT repos with one command"
date: 2015-07-01 00:42:17 +0200
comments: true
categories: 
---

So, now that I have http://git.troglobit.com setup as a backup GIT repo
to https://github.com/troglobit, I needed as *simple* way of always
pushing to both repos -- best way for me is to always hook into my
regular work flow, otherwise I'd forget.  The `git-remote(1)` man page
describes the new `set-url --add` sub-command:

    git remote set-url --add origin troglobit.com:/srv/git/watchdogd.git

Now, with a simple `git push` followed by `git push --tags` I had now
pushed to both the GitHub repo as well as my own server!

To inspect your current push/pull repos, issue `git remote -v`:

    origin	git@github.com:troglobit/watchdogd.git (fetch)
    origin	git@github.com:troglobit/watchdogd.git (push)
    origin	troglobit.com:/srv/git/watchdogd.git (push)

