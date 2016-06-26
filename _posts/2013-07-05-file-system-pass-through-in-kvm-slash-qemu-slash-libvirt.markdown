---
layout: post
title: "File System Pass-Through in KVM/Qemu/libvirt"
date: 2013-07-05 23:56
comments: true
categories:
- Administration
- Linux
---

This post doesn't cover fully setting up KVM/Qemu with virt-manager
and creating virtual machine guests.  See the Ubuntu
[KVM Installtion](https://help.ubuntu.com/community/KVM/Installation),
[VirtManager Guide](https://help.ubuntu.com/community/KVM/VirtManager),
the
[Ubuntu Server Guide on libvirt](https://help.ubuntu.com/13.04/serverguide/libvirt.html),
or
[HowtoForge](http://www.howtoforge.com/virtualization-with-kvm-on-ubuntu-12.04-lts-p3)
for that.

Instead this blog post details the most relevant steps to get file
system pass-through between a Linux host and Qemu guest working.  The
upstream [Qemu docs](http://wiki.qemu.org/Documentation/9psetup)
provide a good starting point, as is the original
[IBM paper on VirtFS](https://www.kernel.org/doc/ols/2010/ols2010-pages-109-120.pdf).
For users of Ubuntu <= 13.04, watch out for the
[libvirt bug](https://bugs.launchpad.net/ubuntu/+source/libvirt/+bug/943680)
that I know many people run into, myself included.

<!-- more -->

First of all, I could never really master the beast called AppArmor in
Ubuntu.  Once I got the hang of the files to edit, the order to make
changes and the syntax of its profile files I think I tried every
possible permutation without any success.  So I ended up disabling the
profile(s) of my VM guests.  The UUID in the filename can be found in
the details of your VM, or in the process listing on the host: `ps fax
| grep guestname`.  Here is an example of how to disable one guest:

	aa-complain /etc/apparmor.d/libvirt/libvirt-20b8c6c6-440c-bd76-254e-42fd475e6770

You need to install [apparmor-utils](apt://apparmor-utils) to get the
`aa-complain` tool.  Where *complain* basically means ignore any hits
from the given profile and just complain in the log.  The default is
`aa-enforce`.  For more info on AppArmor, see the
[excellent upstream docs](http://wiki.apparmor.net/index.php/Documentation)

<img class="right" src="/images/filesystem-pass-through.png" style="width: 500px;">

Now, how to do it.  I like virsh, but for most of the time the vmware
like
[virt-manager is a lot more user friendly](http://virt-manager.org/).
In the VM's Detailed view, click the "Add Hardware" button and select
"Filesystem".  This is where the action happens.

* **Type:** preset to Passthrough
* **Mode:** change to `Mapped` *This is the most important step in
  this blog, or you will not get read/write support!*
* **Source path:** select the path on your host that will be shared
  with this guest.  I use `/var/lib/libvirt/share` but you can use any
  directory you want
* **Target path:** enter magic string that you'll use in the mount
  command in the guest.  I use `share`, no slashes or anything.  In
  reality this isn't a path per se, it's a tag that the guest sends to
  the kernel 9p driver via the mount command

Please note that 9P file systems simply pass-through the owner UID/GID
and directory permissions from the host to the guest.  This can be a
bit confusing, but just make sure to use the same for all guests that
share the same directory.  I chowned it to my account on the host:

	host# chown jocke:users /var/lib/libvirt/share

In my `guest:/etc/modules` I added the following modules, even though
the kernel can probably load them itself on demand:

	9p
	9pnet
	9pnet_virtio

The actual command to get the ball rolling on the guest:

	guest# mount -t 9p -o trans=virtio,version=9p2000.L,rw share /mnt

To automatically mount this every time at boot, add the following
to your `guest:/etc/fstab`:

	share	/mnt	9p	trans=virtio,version=9p2000.L,rw	0	0

That's it. Good Luck!
