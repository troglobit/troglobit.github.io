---
name:  "NetBSD pre-built packages"
title: "HowTo use NetBSD pre-built packages"
date: 2015-07-30 11:13:00 +02:00
aliases: [/howto-netbsd-pkgsrc.html]
categories: [ "netbsd", "bsd", "howto" ]
---

*Reminder to self:* you need to set up the `PKG_PATH` to the correct FTP
URI.  Also, use the correct ARCH, otherwise the installer complains.  Do
not use `amd64`, but rather `x86_64`.

Here it is, you'd think this be something the installer could set up a
default for ...

    PKG_PATH=ftp://ftp.netbsd.org/pub/pkgsrc/packages/NetBSD/amd64/6.1.5/All/

The simply

    pkg_add -v git

Or so you might think!  As [this blog][1] points out, you also need to
install some root certificates to get HTTPS/SSL working. *sigh*

    pkg_add -v openssl
    pkg_add -v mozilla-rootcerts

Followed by

    touch /etc/openssl/openssl.cnf
    cd /etc/openssl/certs
    mozilla-rootcerts extract
    mozilla-rootcerts rehash

Then you can clone your GitHub repo and start working ...

    cd
    git clone https://github.com/troglobit/pimd

On FreeBSD it's a lot simpler, type `pkg dasdastradf` and the OS asks
you nicely if you want to install the pkgsrc system?   Reply and then
simply do `pkg add git`.

[1]: http://www.cambus.net/installing-ca-certificates-on-netbsd/
