---
categories:
- opensource
- ftp
- tftp
- ddns
comments: true
date: 2014-05-20T21:08:25Z
title: New releases of uftpd and inadyn ...
url: /2014/05/20/new-release-of-uftpd-and-inadyn-dot-dot-dot/
aliases: /blog/2014/05/20/new-release-of-uftpd-and-inadyn-dot-dot-dot/
---

The last couple of weeks have both seen the birth of the fabulous
[uftpd](/uftpd.html) and a reignited [inadyn](/inadyn.html) project!
Yesterday v1.2 of uftpd was released and today Inadyn saw the first
working HTTPS support being released as v1.99.8!  This is likely the
last release of Inadyn before the big 2.0, which will introduce the
new .conf file format based on
[libConfuse](http://www.nongnu.org/confuse/).  A .conf file
feasibility study was
[presented earlier](/blog/2014/02/23/weekend-hack-json-vs-conf/) ...

The uftpd project is completely based on user input, currently mostly
mine.  I will add features and bug fixes when I need them, unless I
get more input.  So I welcome anyone to clone it on GitHub and hack
away.  As usual, I will accept any sane patches that adhere to the
coding style and is well written.  The same goes for Inadyn, only with
the added request that the changes are thoroughly tested first, since
I cannot maintain multiple accounts on various DDNS providers myself.

Patches to support flavors of UNIX other than GNU/Linux, including
GNU/Hurd, or even other operating systems are also welcome, as long as
it's not Windows.  I will not maintain any Windows support since I
don't use it myself.  If you want Windows support you will have to
take a very active co-maintainer role in the respective project.
