---
title: "Finit in Linux Containers (LXC)"
date: 2022-02-13 17:55:44 +0200
tags: [ howto, finit, init, opensource ]
draft: false
---

This is a mini-HowTo on running Finit in an LXC system container.  We
will be using a variant (external) of Buildroot, called [NetBox][] to
create a squashfs (read-only) image for the root filesystem.  Then we
will give the container a single writable directory from which it then
uses bind mount to emulate a full-blown system.

It is expected you have LXC installed and all the relevant build tools
needed to create the image.  How to set that up is not covered by this
tutorial.

<!--more-->

First we clone NetBox and build an app image for Zero (x86_64):

```sh
git clone https://github.com/westermo/netbox
cd netbox
make netbox_app_zero_defconfig
make
```

After a few minutes the build has completed and in the `output/images/`
directory we have the `netbox-app-zero.img` which we now can set up an
LXC config file for.  Make sure to create the appropriate directories as
we proceed.  Start by copying the .img file to the LXC images directory,
then create this file:

```conf
# /var/lib/lxc/foo/config 
lxc.uts.name = foo
lxc.tty.max = 4
lxc.pty.max=1024
#lxc.hook.pre-mount = pre-mount.sh /var/lib/lxc/images/foo.img /var/lib/lxc/foo/rootfs
#lxc.rootfs.path    = overlayfs:/var/lib/lxc/foo/rootfs:/var/lib/lxc/foo/delta0
#lxc.rootfs.options = -t squashfs
lxc.rootfs.path = loop:/var/lib/lxc/images/netbox-app-zero.img
lxc.mount.auto = cgroup:mixed proc:mixed sys:mixed
#lxc.mount.entry=run run tmpfs rw,nodev,relatime,mode=755 0 0
#lxc.mount.entry=shm dev/shm tmpfs rw,nodev,noexec,nosuid,relatime,mode=1777,create=dir 0 0
lxc.mount.entry=/var/lib/lxc/foo/mnt mnt none bind 0 0
lxc.net.0.type = veth
lxc.net.0.flags = up
lxc.net.0.link = lxcbr0
#lxc.init.cmd = /sbin/init finit.debug

#lxc.seccomp.profile = /usr/share/lxc/config/common.seccomp
lxc.apparmor.profile = lxc-container-default-with-mounting
```

As can be seen from the example config above, there are many ways to
skin this cat.  This time around are focusing on the uncommented lines
which loop mount the image, mounts a mix of the kernel file systems and
a writable directory on `/mnt` in the container's namespace.  We also
set up basic networking, which will only work if you have the `lxcbr0`
bridge interface on your host system.

Notice the AppArmor profile used, you my need to extend it with the
following extra permissions at the bottom:

```
# /etc/apparmor.d/lxc/lxc-default-with-mounting
mount fstype=tmpfs,
mount fstype=overlay,
```

Reload AppArmor, or restart your system to activate the changes, then we
can start the container with:

```sh
$ sudo lxc-start -n foo -F
● ● ●  NetBox - The Networking Toolbox ═════════════════════════════════
[ OK ] Mounting filesystems
[ OK ] Populating device tree
[ OK ] Restoring system clock (UTC) from RTC
[ OK ] Initializing random number generator
[ OK ] Starting System log daemon
[ OK ] Starting networking
[ OK ] Starting SSH daemon
[ OK ] Calling /etc/rc.local

Welcome to Buildroot
buildroot login: root
                       .--.--.--.-----.-----.-------.-----.----.--------.-----.
   Welcome to NetBox   |  |  |  |  -__|__ --|_     _|  -__|   _|        |  _  |
    Made by Westermo   |________|_____|_____| |___| |_____|__| |__|__|__|_____|
                                                       https://www.westermo.com
NetBox 2021.02-r0-206-g0eba0db-dirty -- Feb 13 19:25 CET 2022
Type: 'help' for help with commands, 'exit' to log out.

root@buildroot:~# 
```

Type `poweroff` to terminate the container.  To start it in the
background, omit `-F` above, then use `lxc-attach` to connect (and
disconnect) from it.

[NetBox]: https://github.com/westermo/netbox
