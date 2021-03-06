---
categories:
- Emacs
- HowTo
comments: true
date: 2008-07-28T01:38:13Z
title: 'HowTo: Build GNU Emacs from CVS'
url: /2008/07/28/howto-build-gnu-emacs-from-cvs/
aliases: /blog/2008/07/28/howto-build-gnu-emacs-from-cvs/
---

Why would you want to do this?  Well, considering all the [neat new things][1]
that have been added lately it should be tempting for any old Emacs fan.

The [Emacs Wiki][2] has all the info you need, but here is a quick run-down of
the bare necessities.  Start by [checking out your working copy][3]:

```
cvs -z3 -d:pserver:anonymous@cvs.savannah.gnu.org:/cvsroot/emacs co emacs
cd emacs/
./configure
make bootstrap
```

Start with `./src/emacs` or symlink the binary to your `~/bin/` directory.
I.e. you don't have to run `make install` to use it.  Users of `emacsclient`
should symlink that to their `~/bin` as well.

Assumptions made:

1. you have the appropriate -dev packages installed in Debian/Ubuntu, and
2. your `.bashrc` does indeed add `~/bin` to your search `$PATH` environment variable

[1]: http://blog.orebokech.com/2008/07/emacs-snapshot-20080727-1.html
[2]: http://www.emacswiki.org
[3]: http://www.emacswiki.org/cgi-bin/emacs-en/EmacsFromCVS
