---
layout: post
title: "VMWare Kernel Modules for Your New Kernel"
date: 2007-03-25 23:59:57 +0200
comments: true
categories: 
---

My recent post on building Debian kernels misses a subtle but useful
point.  Namely that of building Debian kernel modules alongside your new
kernel.  I wanted to run a new kernel with the Ubuntu
vmware-player-kernel-source package.  I was fumbling around wanting to
use `module-assistant` to do this for me when I stumbled upon on the
solution, [presented so eloquently by my friend Albert Veli][1].  This
gives us a small update to the previous post:

```
$ wget ftp://ftp.sunet.se/pub/Linux/kernels/v2.6/patch-2.6.20.4.bz2
$ cd linux-2.6.20/
$ bzcat ../patch-2.6.20.4.bz2 | patch -p1
$ cd ../
$ mv linux-2.6.20 linux-2.6.20.4
$ cd linux-2.6.20.4
$ make oldconfig
$ fakeroot make-kpkg clean
$ fakeroot make-kpkg --initrd kernel_image
$ sudo dpkg -i ../linux-image-2.6.20.4_2.6.20.4-7.troglobit_i386.deb
```

Next up is installing and building the vmware-player-kernel-source:

```
$ sudo apt-get install vmware-player-kernel-source
$ cd /usr/src
$ sudo tar xvfj vmware-player-kernel-source.tar.bz2
$ cd -
$ cd linux-2.6.20.4
$ fakeroot make-kpkg modules_clean
$ fakeroot make-kpkg modules_image
$ sudo dpkg -i  ../vmware-player-kernel-modules-2.6.20.4_2.6.20.4-7.troglobit_i386.deb
```

You may, of course, need to tweak the file `/etc/kernel-pkg.conf`, but
there are man pages for that.

[1]: http://ubuntuforums.org/showthread.php?p=1266060#post1266060
