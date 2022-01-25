---
title: "Buildroot Development Checklist"
orig-date: 2021-01-24 13:26:00 +0100
date: 2022-01-04 10:15:00 +0200
tags: [ buildroot, git, opensource ]
draft: false
---

Because I always forget, here's a reminder to myself on how to use `git
format-patch` and `git send-email` from the command line.

> The following process applies to many other mailing-list based
> projects as well, with local differences in helper scripts.

<!--more-->

The following example is for contributing to the [Buildroot Project][1],
but the process is much the same for other mailing list-based projects.

1. Make your changes on an up-to-date branch from Buildroot master

        $ git checkout -b package/foo
        $ git fetch --all --tags
        $ git rebase origin/master

2. Use *logical commits*; upgrade package as one, changing/extending
   behavior as another, etc.

3. Use commit messages to record *why* changes are made.  First line is
   a summary referencing the sub-system, followed by an empty line and
   the message body, and concluded by your sign-off:
   
        package/foo: bump version to v1.2.3

        Signed-off-by: Your Name <your.name@example.com>

4. Verify formatting of package files; .in, .mk, etc.

        $ ./utils/check-package package/foo/*

   If you change or add a new package, verify you don't introduce any
   recursive dependencies, or other nasty surprises.  Remember, the
   build systems includes *all* `.mk` files into one big Makefile, so
   any changes you make are seen by *all* other packages.

        $  make menuconfig         # complains if you have recursive deps.
        $  make <pkg>-rebuild      # check output of build

5. Test your package/change with a set of cross-compilation toolchains.
   The `.config` file is a menuconfig snippet enabling the package to
   test:

        $ ./utils/test-pkg -c foo.config -p foo

   Output goes in `~/br-test-pkg/`, check all logs.  Some toolchains
   can help you track down issues with your package you won't see
   elsewhere.

6. Format your patches, with the optional `--cover-letter`, very useful
   to explain a series of patches:

        $  git format-patch --cover-letter -M -n -s -o mail origin/master

   Remember to **edit the cover letter** --- it serves as an
   introduction and explains the reasoning behind your changes.  Focus
   on *the why, not the how,* the patches have separate commit messages

7. Figure out DEVELOPERS to Cc in your correspondence to the mailing list:

        $ ./utils/get-developers mail/*
        git send-email --to buildroot@buildroot.org --cc foo@example.com

8. At the very least, the following should be output:

        git send-email --to buildroot@buildroot.org
		
9. Append `mail/*` to send.  You and others in Signed-off-by,
   Reviewed-by: are Cc:ed by default

        $ git send-email --to --cc foo@example.com buildroot@buildroot.org mail/*
		
   You are offered one last chance to proofread the contents (remember
   to check the email headers!) before you send.

> **Note:** if you haven't set up your `~/.gitconfig` yet for sending
> email, please see https://git-scm.com/docs/git-send-email or my blog
> post https://troglobit.com/post/2022-01-24-gmail-and-git-send-email/
> for details.
