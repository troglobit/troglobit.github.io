---
layout: post
title: "Merecat - another stray kitten?"
date: 2016-11-06 01:34:18 +0100
comments: true
categories:
---

I code for recreation as well as work.  Admittedly I've got a touch of
the NIH flu, though most of the time I tinker around with my various
[projects](https://github.com/troglobit) simply to learn.  Sometimes
these little projects turn into something useful for other people as
well, which is great!

Recently I discovered my method to get started: refactoring, or just
simple code cleanup.  You see I've got this crazy idea that *all simple
things are correct*.  Although things usually tend to require a certain
balance -- not all things can be simplified, and not all simple things
are correct.

Anyway, I strive for this every day: learning and simplicity.  So when I
recently had to migrate my personal website and some other misc. sites,
I first set out to run everything from an old RasPi2 I had.  This put my
private life in just about the same niche as my work life, embedded.

So I set out to (re-)discover the glorious little web servers of my past
I had almost forgotten: [Boa](http://www.boa.org/),
[mini_httpd](http://acme.com/software/mini_httpd/), BusyBox httpd,
[thttpd](http://www.acme.com/software/thttpd/), and more ... come to
think of it, they are probably one of the many reasons that I started my
career in embedded in the first place!

This post is about my adoption, refactor, and rebranding of thttpd as
[Merecat](http://merecat.troglobit.com).

<!-- more -->

With all the warm and fuzzy memories I had of thttpd I decided to give
it a whirl.  And with my usual lack of insight (of how much time this
would consume) I started fixing bugs and issues that nagged me.

Ten patches in I hesitated and set out on another archaeological
expedition, surely I couldn't have seen all these problems myself?  The
usual burial grounds for ancient software were visited: *BSD, Debian,
Gentoo, SourceForge, Internet Archive ... that's when I found
[sthttpd](https://github.com/blueness/sthttpd/) by Anthony G. Basile.

Anthony had left sthttpd in great shape!  Not only had he merged all
Gentoo fixes, he had also converted the build system to GNU configure
and automake.  So I started rebasing my patches on sthttpd and then I
simply couldn't stop.  The refactoring machine in my head was on fire, I
simpy had to let it all out!

The goal I had was a simple to use, good looking by default, bare bones
web server.  My use-case was simple: serve gitweb, HTTP gateway for my
FTP, serve my resume, and possibly even my Jekyll blog.  So I basically
just needed to get virtual hosts and dir listings working.

In the process, however, I had done a lot more. Lots of half baked ideas
and barely working code.  But it worked for me, so I tagged the first
release of Merecat, v2.30, and went on my merry way.

Until the RasPi server crashed.  I thought it might be heat and maybe
overclocking issues, so I moved it to my server room and downclocked it.
A few weeks passed and everything seemed to work just fine, then bam!
Another crash, and now it only stayed up a few hours at a time between
crashes.

Heartbroken (well not really) I decided it was time to move to the cloud
like everyone else.  So a droplet was created (clear of US influence I
hope) and data was being moved to the new location.  This time I wanted
the setup to be as clean as possible, so only Debian packages and proper
systemd integration.  Fortunately I had a well timed week off from work
to do all this.

I had just barely started digging in, when I got a bug report by email
from Gaetan Bisson who had found lots of issues with Merecat.  The bugs
he reported overlapped nicely with the work I planned to do anyway \o/

The resulting v2.31 release of Merecat has a lot of fixes and new
features, these are the ones I can remember (and are in the GitHub
changelog):

### Changes

- Sort directories first in dir listings
- Include systemd unit file
- Add `debian/` packaging
- Add `--enable-public-html` to enable `~user/public_html` dirs
- Add support for using top-level `/cgi-bin` as fallback from vhosts
- Update default landing page

### Fixes

- Add missing CSS and jpeg files to install
- Fix dependency tracking when reconfiguring
- Fix .conf file parser bugs reported by Gaetan Bisson
- Fix missing `HAVE_LIBCONFUSE` #define causing .conf file support to
  not be built, reported by Gaetan Bisson
- Fix malplaced call to `cfg_free()` in .conf file parser, reported by
  Gaetan Bisson
- Update man page and other documentation with missing quotes around CGI
  pattern, issue reported by Gaetan Bisson
- Fix syslog warning: bind 0.0.0.0: Address already in use 

Download the release from my [FTP](http://ftp.troglobit.com), or the
[GitHub release page](https://github.com/troglobit/merecat/releases/tag/v2.31),
which also has a pre-built .deb for Ubuntu 16.04.  For those who want to
build the .deb themselves:

    cd merecat/
    dpkg-buildpackage -uc -us -B -d

Then simply:

    sudo dpkg -i ../merecat_2.31-3_amd64.deb

**Note:** For use with GitWeb, remember to install `libcgi-pm-perl`

I'd like to take this opportunity to thank a couple of friends who
helped out with the auditing and layout testing of the new default
landing page.  Thank you Anders Bornäs and Martin Olsson :-)

<!--
  -- Local Variables:
  -- mode: markdown
  -- End:
  -->
