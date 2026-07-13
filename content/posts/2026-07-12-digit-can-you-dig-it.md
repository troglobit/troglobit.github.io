---
title: "Can You Dig It?"
subtitle: "A fast little web frontend to git — v1.0, with v1.1 a day later"
date: 2026-07-12T20:30:00+02:00
lastmod: 2026-07-13T16:43:27+02:00
tags: [ digit, git, go, opensource, selfhosting ]
image: /images/digit-card.png
description: "Announcing Digit, a fast and simple web frontend to git repositories.  Born the day an AI scraper herd melted my droplet, it browses mirror trees, renders GitHub-grade markdown, and serves crawlers nothing but 304s.  Live at https://git.troglobit.com"
---

{{% figure src="/images/digit-1.0.png" class="right-floated" width="512" alt="Digit repo overview" link="https://git.troglobit.com" %}}

Like many of you I keep mirrors of my projects on a small VPS —
partly as insurance, GitHub is lovely until it isn't, and partly
because I like having my own corner of the web.  For years [gitweb][]
did the honors, showing its age but doing the job.  When I went
looking for something nicer I found [legit][], which looks great —
but on my single-core droplet it *pegged the CPU* just rendering a
repo page.  Add the AI scraper herds that graze on every public git
frontend these days, and the poor machine never had a quiet moment.

<!--more-->

So, obviously, the reasonable thing to do was to write my own.  Say
hello to **[digit][]**, which just shipped ~~[v1.0][]~~[v1.1][].

### Display Git

The name came first, I'll admit it.  *Digit* — display git.  Dig it?
It also means finger, so it keeps a finger on your repos.  The puns
run deep, and picking the heading for the README was among the harder
design decisions of the project.

Under the hood the design is old-school: digit is a single static Go
binary that shells out to the real `git` for everything, just like
gitweb and cgit before it.  The git binary has decades of packfile
and commit-graph optimizations that pure-Go re-implementations simply
lack, that's the difference between browsing a kernel-sized repo and
melting a droplet.  On top of that sits an aggressive cache: every
page carries an ETag derived from the repo's actual ref state,
hash-pinned URLs are marked immutable forever, and the scraper herds
that inspired the whole exercise now graze on a diet of 304s.  A
bundled `robots.txt` and fail2ban-friendly logs handle the ones that
don't take the hint.

### What It Does

Everything you'd expect from the classic tier (trees, logs, diffs,
blame, refs, snapshots) plus the things I always missed there:

 - **Mirror-tree aware:** repos nested `host/org/repo.git`, the way
   mirror scripts lay them out, get grouped per org with index pages
   at every level.  My whole `/srv/git` just works.
 - **GitHub-grade markdown:** emoji, tables, alerts (`> [!TIP]`),
   relative links and images, plus readme tabs for license,
   contributing, and friends.  Your project pages look like project
   pages, not like 2004.
 - **Read-only clone over smart HTTP:** with a GitHub-style clone
   menu, copyable HTTPS/SSH URLs and tar.gz/zip snapshots.  Handy
   for an office-internal instance, off by default for public ones.
 - **Atom and RSS feeds**, per repo and site-wide, commit message
   search, `.patch` URLs, dark mode with a proper toggle, and line
   anchors with GitHub-style visual range marks:

{{% figure src="/images/digit-1.0-marks.png" class="center" width="560" caption="Shift-click the gutter for #L10-L20 links with a visual mark" %}}

Features that cost CPU, bandwidth, or attack surface (blame, snapshots,
clone, search) sit behind config toggles, cgit style, so every instance
is exactly as wide as its operator wants.

And it hasn't stopped at the first release.  The very next day
**[v1.1][]** landed, a mobile and accessibility pass.  On a phone the
whole thing folds down now: a hamburger swallows the repo tools, and the
file and commit views get tidy framed headers.  It behaves with a
keyboard and a screen reader, and the log grew little tag and branch
pills so you can spot where a release landed.  Keyboard navigation still
hops between a commit and its parents or children and walks the log
without touching the mouse; tap `?` for the map.

### Grab It

Building is the boring kind of boring:

```bash
git clone https://github.com/troglobit/digit.git
cd digit
make
sudo make install
```

Prebuilt static binaries for linux amd64/arm64 hang off the [v1.1
release][v1.1], and the [README][digit] covers configuration,
reverse proxying with nginx or [merecat][], and running under systemd
or [Finit][].

Best of all: you can see it in action right now, browsing its own
source, at **[git.troglobit.com][live]**.  That site is a small
experiment in its own right,  it's served through [merecat][]'s
brand-new `proxy-pass`, as far as I know the first service anywhere
running on it, which quietly makes git.troglobit.com equal parts git
browser and merecat proving ground.  Two birds, one droplet.

Bug reports and patches welcome, as always.  Can you dig it?

[gitweb]:   https://git-scm.com/docs/gitweb
[legit]:    https://github.com/icyphox/legit
[digit]:    https://github.com/troglobit/digit
[v1.0]:     https://github.com/troglobit/digit/releases/tag/1.0
[v1.1]:     https://github.com/troglobit/digit/releases/tag/1.1
[merecat]:  https://github.com/troglobit/merecat
[Finit]:    https://github.com/troglobit/finit
[live]:     https://git.troglobit.com/github.com/troglobit/digit/
