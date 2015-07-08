---
layout: post
title: "HowTo: Converting from Bazaar to GIT"
date: 2010-07-13 01:56:11 +0200
comments: true
categories: 
- Bzr
- Git
- HowTo
---

You need git, and bazaar obviously.  Also install bzr-fastimport, it
contains the export plugin as well. The rest is a rip off from the
folloing URL: http://fthieme.net/en/drupal6/node/77

    git init project.git
    cd project.git
    bzr fast-export --export-marks=.git/bzr.mark ~/project.bzr | git fast-import --export-marks=.git/git.mark

That worked for me. The output will likely be something like this:

    01:41:19 Calculating the revisions to include ...
    01:41:19 Starting export of 33 revisions ...
    01:41:20 Exported 33 revisions in 0:00:01
    git-fast-import statistics:
    ---------------------------------------------------------------------
    Alloc'd objects:         5000
    Total objects  :          267 (         0 duplicates                  )
            blobs  :          158 (         0 duplicates         57 deltas)
            trees  :           76 (         0 duplicates         55 deltas)
            commits:           33 (         0 duplicates          0 deltas)
            tags   :            0 (         0 duplicates          0 deltas)
    Total branches :           10 (         1 loads     )
            marks  :         1024 (        33 unique    )
            atoms  :           38
    Memory total   :         2344 KiB
            pools  :         2110 KiB
            objects:          234 KiB
    ---------------------------------------------------------------------
    pack_report: getpagesize()            =       4096
    pack_report: core.packedGitWindowSize = 1073741824
    pack_report: core.packedGitLimit      = 8589934592
    pack_report: pack_used_ctr            =        267
    pack_report: pack_mmap_calls          =         73
    pack_report: pack_open_windows        =          1 /          1
    pack_report: pack_mapped              =     977484 /     977484
    ---------------------------------------------------------------------

Now just do a simple `git checkout master` to get started working again.
