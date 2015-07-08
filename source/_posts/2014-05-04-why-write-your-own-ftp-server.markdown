---
layout: post
title: "Why write your own FTP server?"
date: 2014-05-04 11:25:09 +0200
comments: true
categories: 
- opensource
- ftp
---

So, I finally got fed up with all other FTP servers and wrote
[my own](/uftpd.html).  Why would someone in their right mind do
something like this 2014?

As a developer the answer to most such questions is usually; to
scratch an itch.  For a very long time I've looked for a really simple
FTP server that just works, out of the box!

<!-- more -->

My alternatives on GNU/Linux distributions have been: several variants
of the original [MIT/Athena ftpd](https://github.com/mit-athena/ftpd),
[ProFTPD](http://www.proftpd.org/),
[Pure-FTPd](http://www.pureftpd.org/project/pure-ftpd), and
[vstfptd](https://security.appspot.com/vsftpd.html).  Most of them are
too damn difficult to configure, have ugly configuration files or are
plainly just too confusing.  With one exception, vsftpd.  I really
like vsftpd!  It's got a simple and well documented configuration file
(inline as comments) and always worked almost painlessly for me.

However, over the past couple of Ubuntu LTS installations it's been
starting to act up on me.  Not allowing me to have a group-writable
root directory, and not even an ability to disable that security
feature.

It's simple really.  I'm a developer.  I have colleagues.  We share
servers at work.  We bootstrap and upgrade our embedded devices using
TFTP/FTP.  We have one directory, usually `/srv/ftp`, which is group
writable and all developers are members of that group.  Anyone can
upload a file there and the file servers ... would you know ... are
supposed to *serve* that file, as TFTP or anonymous FTP.

Maybe I'm a fringe user?  Maybe secured local area networks, or laptop
to embedded device crossed-cables networks, are no longer the "fad"?
Maybe a sane built-in default is no longer cool or hipster enough?

I don't know anymore, and this time I was so fed up chasing around for
answers to "How to setup your 'favorite' FTP Server", that I just sat
down and fixed it once and for all.  I figured I'd otherwise be doing
this waltz over and over again, just like I already have, for the next
10 years ...

I trawled the net once more for good candidates, this time for
adoption and holy cow sacrifices.  I wanted a very simple base to
start from, which I could cut up into pieces, iteratively refactor and
improve upon until I was happy.  That took a while.

I'm a big fan of both the Free Software and the Open Source cultures.
(They are slightly different you know.)  I routinely work on software
with GNU GPL, MIT/X11, Apache, and ISC licenses, but for most of my
own creations I usually lean towards the very permissive
[ISC license](http://en.wikipedia.org/wiki/ISC_license).  Maybe
because most of my hobby work these days are quick and dirty hacks I
don't really care enough about.  It's a question of both taste and
appropriateness -- like software patents, I don't like them either,
but for anything which I have a great investment in, or has a great
[threshold of originality](http://en.wikipedia.org/wiki/Threshold_of_originality),
I'd use a more restrictive license to make sure the software is kept
in the open.

Anyway, I read much code, discarded several projects that were too
big, too unreadable code or simply didn't feel right.  I tested so
many small "FTP" servers that turned out to be unusable homegrown
variants or school projects.  There's a lot of code out there ...  I
finally settled on [FtpServer](https://github.com/xu-wang11/FtpServer)
by [Xu Wang](https://github.com/xu-wang11) as a base for my little
project.  I suspect it too is a school project, but this one actually
worked, right off the bat!

I got so excited that I immediately started improving it, cleaning up
the source code, reindenting it (KNF) and fixing small bugs here and
there.  It was a perfect fit!  This is where I made my big mistake,
which I hope will not eventually kill this project in the end -- the
FtpServer project did not have a license ... so
[I filed a bug](https://github.com/xu-wang11/FtpServer/issues/1) to
the GitHub issue tracker, added the ISC license to
[my fork](https://github.com/troglobit/uftpd) and continued hacking
away.  The changes I've made are so substantial that there isn't much
left of the original code, so uftpd should be safe.  Yes, I named it
uftpd, the micro ftp server -- the no nonsense file transfer protocol
daemon.

The whole hack took about three days and I've learned a lot during
this time!  It was fun, inspiring and gave me a lot of creative energy
that I can use in other projects: at work as well as at home :)

See the [uftpd project home](/uftpd.html) for download links.
