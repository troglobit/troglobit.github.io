---
categories:
- HowTo
- Git
comments: true
date: 2015-07-01T00:42:17Z
title: 'HowTo: Push to multiple GIT repos with one command'
url: /2015/07/01/howto-push-to-multiple-git-repos-with-one-command/
aliases: /blog/2015/07/01/howto-push-to-multiple-git-repos-with-one-command/
---

So, now that I have http://git.troglobit.com setup as a backup GIT repo
to https://github.com/troglobit, I needed a *simple* way to always push
to both repos -- best way for me is to always hook into my regular work
flow, otherwise I'd just forget.  The [git-remote(1)](1) man page to the
rescue, it describes the `set-url --add` sub-command:

    git remote set-url --add origin ssh://git.troglobit.com:1234/srv/git/watchdogd.git

Now, with a simple `git push` followed by `git push --tags` I had now
pushed to both the GitHub repo as well as my own server!

<!--more-->

Obviously I don't use port 1234, but that's the syntax if you want to
push to a GIT server over SSH on a different port than the default (22).

You can of course just edit the `.git/config` file instead, which in
many cases might be faster:

    [remote "origin"]
        fetch = +refs/heads/*:refs/remotes/origin/*
        url = git@github.com:troglobit/finit.git
        url = ssh://git.troglobit.com:1234/srv/git/finit.git

Of course I first had to create the empty `watchdogd.git` on the server.
I do this quite often, so I've wrapped it in a `makerepo.sh` script:

    cd /srv/git
    git init --bare watchdogd.git
    echo "Refurbished watchdog daemon from uClinux-dist" >watchdogd.git/description

To inspect your current push/pull repos, issue `git remote -v`:

    origin	git@github.com:troglobit/watchdogd.git (fetch)
    origin	git@github.com:troglobit/watchdogd.git (push)
    origin	ssh://git.troglobit.com:1234/srv/git/watchdogd.git (push)

[1]: http://git-scm.com/docs/git-remote
