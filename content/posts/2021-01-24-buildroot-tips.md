---
title: "Buildroot Development Checklist"
orig-date: 2021-01-24 13:26:00 +0100
date: 2021-10-09 10:15:00 +0200
tags: [ buildroot, git, opensource ]
draft: false
---

Because I always forget, here's a reminder to myself on how to use `git
format-patch` and `git send-email` from the command line.

<!--more-->

The following example is for contributing to the [Buildroot Project][1],
but the process is much the same for other mailing list-based projects.

1. Make your changes on an up-to-date branch from Buildroot master

        git checkout -b package/foobar
        git fetch --all --tags
        git rebase origin/master

2. Use *logical commits*; upgrade package as one, changing/extending
   behavior as another, etc.

3. Use commit messages to record *why* changes are made.  The first
   line is usally a (very) brief summary referencing the sub-system:
   
        package/foobar: bump version to v1.2.3

        Signed-off-by: Your Name <your.name@example.com>

4.  Verify formatting of package files; .in, .mk, etc.

        ./utils/check-package package/foobar/*

5. Test your package/change with a set of cross-compilation toolchains.
   The `.config` file is a menuconfig snippet enabling the package to
   test:

        ./utils/test-pkg -c foobar.config -p foobar

6. Format your patches, with the optional `--cover-letter`, very useful
   to explain a series of patches:

        git format-patch --cover-letter -M -n -s -o mail origin/master

7. Figure out DEVELOPERS to Cc in your correspondence to the mailing list:

        ./utils/get-developers mail/*

8. At the very least, the following should be output:

        git send-email --to buildroot@buildroot.org
		
9. Send the email(s) by copy-pasting the output and appending `mail/*`

        git send-email --to buildroot@buildroot.org mail/*
		
   Git now adds a Cc to you and offers you one last chance to proofread
   the contents (remember the email headers!) before you send.

> **Note:** if you haven't set up your `~/.gitconfig` yet for sending
> email, please see https://git-scm.com/docs/git-send-email for help.
