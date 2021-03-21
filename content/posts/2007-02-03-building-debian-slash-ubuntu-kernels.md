---
categories:
- kernel
- ubuntu
- debian
- HowTo
comments: true
date: 2007-02-03T11:00:45Z
title: 'HowTo: Building Debian/Ubuntu Kernels'
url: /2007/02/03/building-debian-slash-ubuntu-kernels/
aliases: /blog/2007/02/03/building-debian-slash-ubuntu-kernels/
---

I have been over this topic so many times now, strangely enough I've
managed to make things more complicated than they need to be.  It's
really this simple:

```
$ wget ftp://ftp.sunet.se/pub/Linux/kernels/v2.6/linux-2.6.19.2.tar.bz2
$ tar xfj linux-2.6.19.2.tar.bz2
$ cd linux-2.6.19.2/
$ zcat /proc/config.gz >.config
$ make menuconfig
[Tweak to your hearts desire]
$ fakeroot make-kpkg --initrd kernel_image
$ dpkg -i ../linux-image-2.6.19.2_2.6.19.2-10.00.Custom_i386.deb
[DONE!]
```

You may, of course, need to tweak the file `/etc/kernel-pkg.conf`, but
there are man pages for that.
