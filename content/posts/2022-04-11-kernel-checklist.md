---
title: "Linux Kernel Development Checklist"
date: 2022-04-11 17:19:00 +0200
tags: [ linux, kernel, git, opensource ]
draft: false
---

Because I always tend to forget steps, and haven't half replaced myself
with a shell script (yet), here's a reminder to myself on how to post a
patch to the [Linux netdev](https://lore.kernel.org/netdev/) mailing
list.

<!--more-->

1. Make your changes on an up-to-date branch from net-next master:

        $ git checkout -b my-patch-series
        $ git fetch --all --tags
        $ git rebase net-next/master

2. Use *logical commits*; upgrade package as one, changing/extending
   behavior as another, etc.

3. Use commit messages to record *why* changes are made.  First line is
   a summary referencing the sub-system, followed by an empty line and
   the message body, and concluded by your sign-off:
   
        net bridge: add support for foo bar
        
        You need an actual body of your commit message, otherwise the
		checkpatch.pl script (below) will complain.  For good reason,
		why wouldn't you want to tell the tale of how you ended up at
		this point in history?
        
        Signed-off-by: Your Name <your.name@example.com>

4. Maybe consider adding a selftest.  Better than having to answer any
   question later on how your feature is supposed to work.  Or even to
   give a maintainer a before-after test to see your patch actually 
   does fix what you say it does.

5. Format your patches, with the optional `--cover-letter`, very useful
   to explain a series of patches:

        $  git format-patch --cover-letter -M -n -s -o mail net-next/master

   Remember: **edit the cover letter** --- it serves as an introduction
   and explains the reasoning behind your changes.  Focus on *the why,
   not the how,* the patches have separate commit messages

6. Check your patch(es), this is one of the most important steps.  The
   `checkpatch.pl` script is the collected wisdom of the ancients, the
   oracle that you can ask free of shame before submitting yourself to
   the wrath of the maintainers:

        $ ./scripts/checkpatch.pl mail/*

7. Figure out maintainers to Cc in your correspondence to the mailing
   list.  Please note, it is *up to you* to figure out the relevant
   people and lists to Cc.  The script gives you an idea of who have
   recently traveled beyond the rim like you, as well as those who do
   live there permanently.

        $ ./scripts/get_maintainer.pl mail/*

9. Inspect the cover letter and patches one last time:

   * Did you remember to add all `Reviewed-by:` and `Acked-by:` tags
     from a previous version you sent?
   * Did you double-check any updated email addresses for maintainers?
   * OK, time to send ...

        $ git send-email --to netdev@vger,kernel.org --cc foo@example.com mail/*

   You are offered one last chance to proofread the contents (remember
   to check the email headers!) before you send.

> **Note:** if you haven't set up your `~/.gitconfig` yet for sending
> email, please see https://git-scm.com/docs/git-send-email or my blog
> post https://troglobit.com/post/2022-01-24-gmail-and-git-send-email/
> for details.
