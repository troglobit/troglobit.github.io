---
categories:
- Administration
- Linux
comments: true
date: 2013-07-10T00:00:00Z
title: Net Install CentOS
url: /2013/07/10/net-install-centos/
---

I usually run Debian or Ubuntu on my machines.  However, having
recently found some time to work on my various projects again, I've
now suddenly found myself in need of a CentOS machine.

The [CentOS home page](http://www.centos.org/) invited me to download
an installation ISO, so I went for the small
[Net Install](http://ftp.funet.fi/pub/mirrors/centos.org/5.9/isos/x86_64/CentOS-5.9-x86_64-netinstall.iso)
which started perfectly with my virt-manager in Ubuntu.

All I had to provide was an FTP server and directory:

* mirrors.kernel.org
* /centos/5.9/os/x86_64
* Anonymous FTP

That's it, the graphical installer started and I had to start
selecting various packages. Must say it's a bit confusing since the
package naming is not the same in RedHat/CentOS as in Debian.

Oh, and if the installations seems to have gotten stuck, just wait it
out.  It'll get there :)

Read the following to get
[console in virsh working with CentOS guest](http://prefetch.net/blog/index.php/2009/06/17/redirecting-the-centos-and-fedora-linux-console-to-a-serial-port-virsh-console-edition/).
