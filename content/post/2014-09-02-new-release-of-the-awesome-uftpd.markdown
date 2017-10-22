---
categories:
- opensource
- ftp
- tftp
comments: true
date: 2014-09-02T10:47:35Z
title: The awesome uftpd, now w/ TFTP support! :)
url: /2014/09/02/new-release-of-the-awesome-uftpd/
aliases: /blog/2014/09/02/new-release-of-the-awesome-uftpd/
---

Today sees the release of v1.3 of the awesome little uftpd. The main
news is the new TFTP support!  Just like before you don't need any
configuration file, just build and install -- or build a .deb file and
install.

This release completes the main purpose of uftpd for me, I can now use
it as my daily driver and fully replace vsftpd and tftpd-hpa, which to
me are the next best.

What's left then, you ask?  Well, TFTP upload support, TFTP segment
size negotiation and possibly multiple user support in FTP.  As
always, patches are most welcome.  See the
[uftpd homepage](/uftpd.html) and my
[GitHub project](https://github.com/troglobit/uftpd/) for details.
