---
categories:
- Gnome
comments: true
date: 2007-07-07T09:52:17Z
title: Buggy Gnome Main Menu
url: /2007/07/07/buggy-gnome-main-menu/
---

I've been living with the new Gnome Main Menu for a while now.  Like many
others I run Ubuntu these days and I've found the version in Feisty Fawn
to be extremely slow and buggy.

I tried manually installing (`dpkg -i`) the latest version in Gutsy, but
it had too many dependencies.  So I decided to try and backport it, and
it worked!

I haven't set up any repository, so just download the below two `.deb`
files and install them with `sudo dpkg -i *.deb` in a terminal window:

* [libslab-0.9.8.deb](http://vmlinux.org/jocke/debian/libslab0_0.9.8.svn.20070430-1ubuntu1_i386.deb)
* [gnome-main-menu-0.9.8.deb](http://vmlinux.org/jocke/debian/gnome-main-menu_0.9.8.svn.20070430-1ubuntu1_i386.deb)

The `md5sums` for these files are:

    3f0a962ffdd2974663f2f425eed5f541   libslab0_0.9.8.svn.20070430-1ubuntu1_i386.deb
    1d11fcace33bab707e28c230c85ea1ba   gnome-main-menu_0.9.8.svn.20070430-1ubuntu1_i386.deb
