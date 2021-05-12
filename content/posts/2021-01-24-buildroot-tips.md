---
title: "Buildroot Development Checklist"
orig-date: 2021-01-24 13:26:00 +0100
date: 2021-05-11 18:41:00 +0100
tags: [ buildroot, git, opensource ]
draft: false
---

Because I always forget, here's a reminder to myself.

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

        git format-patch --cover-letter -M -n -s -o outgoing origin/master

7. Figure out DEVELOPERS to Cc in your correspondence to the mailing list:

        ./utils/get-developers outgoing/*

8. At the very least, the following should be output:

        git send-email --to buildroot@buildroot.org --cc "Your Name <your.name@example.com>"

