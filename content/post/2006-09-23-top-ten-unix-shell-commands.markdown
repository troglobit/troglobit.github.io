---
categories: null
comments: true
date: 2006-09-23T23:02:42Z
title: Top Ten UNIX Shell Commands
url: /2006/09/23/top-ten-unix-shell-commands/
aliases: /blog/2006/09/23/top-ten-unix-shell-commands/
---

Current trend is to run the following
[one-liner from IBM](http://www-128.ibm.com/developerworks/aix/library/au-productivitytips.html?ca=dgr-lnxw07UNIX-Office-Tips).
I'm usually logged in to the following three systems, with very
different results.


vmlinux.org
-----------

    $ history |awk '{print $2}'|sort|uniq -c|sort -nr|head -10
    144 ls
     61 cd
     58 sudo
     29 less
     23 emacs
     19 bzr
     16 vim
     11 rm
     10 wget
     10 mv

My personal life.  Not much different from my professional dito.  Most
visible difference maybe is my use of bzr instead of svn.  At the time
we decided on svn at work bzr had not matured enough and darcs didn't
(still doesn't) have a Windows client as good as TortoiseSVN.


Build server @work
------------------

    $ history |awk '{print $2}'|sort|uniq -c|sort -nr|head -10
    108 cd
     63 svn
     44 search
     41 ls
     25 sudo
     23 less
     23 ./build.sh
     21 make
     17 ./netprobe
     16 man

Mostly work stuff,
['search'](https://github.com/troglobit/toolbox/blob/master/search) is a
small wrapper around find that skips .svn dirs and semantic.cache files
and other annoying things you constantly run in to when looking for
something.  The build script is a small wrapper around make for our
Snapgear and uClinux-dist trees. Netprobe was one of the last prototypes
I wrote to investigate some UNIX peculiarities involving libnet and
libpcap.


troglobit (my laptop)
---------------------

    $ history |awk '{print $2}'|sort|uniq -c|sort -nr|head -10
    140 sudo
     47 ifconfig
     45 less
     27 ps
     26 ssh
     25 iwconfig
     21 ping
     21 man
     15 ls
     15 cd

Well, I'm having trouble with my laptop network. The wlan
(wpa_supplicant) doesn't like suspend/resume on my ThinkPad T43.

*Update:* I had to make an update to this, even at first I had my doubts
 about the first script. It didn't feel right, now after revising it,
 and reading
 [Planet Debian](https://web.archive.org/web/20090601001529/http://planet.debian.org/),
 I noticed Erich Schuberts entry.  There it was, the working script,
 which I only had to bashify!  Thanks Erich!
