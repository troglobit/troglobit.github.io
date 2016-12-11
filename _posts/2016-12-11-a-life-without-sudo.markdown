---
layout: post
title: "A life without sudo"
date: 2016-12-11 20:26:12 +0100
comments: true
categories:
- sudo
- sandwich
- linux
- capabilities
---

Ever since my first stumbling steps with Linux back in '96, I've been
learning about UNIX.  The first obvious lesson was to not use the root
account.  Since then I've been using a combination of `sudo command` and
suid root binaries to get the job done.

[![sudo make me a sandwich](/images/sandwich.png)](https://xkcd.com/149/)

For the last ten years, however, I've been meaning to learn about Linux
[capabilities(7)](http://man7.org/linux/man-pages/man7/capabilities.7.html)
and thanks to a collegue of mine I now have :)

What you want is to grant capabilities per user *and* application.  Most
tutorials only tell you how to do one or the other.

First of all you need to figure out what capabilities an application
requires to perform an action.  Let's use `tcpdump` as an example, it
needs raw link access to sniff packets so your user (you), need to be
listed in the system `/etc/security/capability.conf` file:

    cap_net_raw     joachim

Second, you need to set this on the application, so that when `joachim`
wants to run `tcpdump` he is granted the capability:

    $ sudo /sbin/setcap cap_net_raw+ep /usr/sbin/tcpdump

Some applications require multiple capabilities, like Qemu when you use
tap networking.  Update `/etc/security/capability.conf`

    cap_net_raw     joachim
    cap_net_admin   joachim

Then add both capabilities to qemu, like this:

    $ sudo /sbin/setcap cap_net_raw,cap_net_admin+ep /usr/bin/qemu-system-arm

<!--
  -- Local Variables:
  -- mode: markdown
  -- End:
  -->
