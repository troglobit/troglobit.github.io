---
categories:
- Finit
- opensource
comments: true
date: 2015-01-24T16:54:23Z
title: Finit v1.11 released!
url: /2015/01/24/finit-v1-dot-11-released/
---

**Update 2015-03-09**: This release has unfortunately been *yanked* due
to serious regressions in launching background processes.  It has been
replaced by [v1.12]

<!--more-->

I'm very pleased to announce [Finit v1.11](/finit.html) — this is the
episode where two of my projects finally get married! ツ

### Changes
* Now using the asynchronous [libuEv] library for handling all events:
  signals, timers and listening to sockets or file descriptors.
* Rename NEWS.md --> CHANGELOG.md, with symlinks for `make install`
* Attempt to align with http://keepachangelog.com for the ChangeLog
* [Travis CI] now only invokes [Coverity Scan] from the 'dev' branch.  This
  means that all development, except documentation updates, must go into
  that branch.

### Fixes
* Fix bug with finit dying when no `tty` is defined in `finit.conf`, now
  even the fallback shell has control over its TTY, see fix in GIT
  commit [dea3ae8] for this.

[v1.12]: /blog/2015/03/06/inetd-support-in-finit-v1-dot-12/
[libuEv]: https://github.com/troglobit/libuev
[Travis CI]: https://travis-ci.org/troglobit/finit
[Coverity Scan]: https://scan.coverity.com/projects/3545
[dea3ae8]: https://github.com/troglobit/finit/commit/dea3ae8
