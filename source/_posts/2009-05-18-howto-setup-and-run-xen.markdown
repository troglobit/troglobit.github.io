---
layout: post
title: "Howto Setup and Run Xen"
date: 2009-05-18 00:51:42 +0100
comments: true
categories: 
---

This is an extremly brief and quick [Xen](http://www.xenproject.org/)
tutorial. There are lots of them already, see your <DISTRIBUTION> wiki,
HowtoForge or other places for a starter guide. This particular HowTo
deals with setting up Xen as easy as possible using Ubuntu 8.04 LTS as
host operating system (dom0 in Xen terms) and Ubuntu 9.04 as guest
operating systems (domU in Xen lingo).

Ubuntu 8.04 LTS comes with a Xen kernel that can run as dom0. This is
quite important, so pay attention to setting that up correctly. For this
I recommend the
[HowtoForge Tutorial](http://howtoforge.org/high-performance-xen-on-ubuntu-8.04-amd64).

*Note:* you will need to upgrade the default Xen 3.2 to 3.3 to be able
to run the latest distributions as guest OS. See this
[Ubuntu question](https://answers.launchpad.net/ubuntu/+question/50326)
for more insight. To get Xen 3.3, simply activate the Hardy backports
repository in your `/etc/apt/sources.list` and, as usual, pay attention
to your `/boot/grub/menu.lst` so that it points out the Xen 3.3 image in
the `defaults=` option.

When done with the basic setup you should install pygrub on your host,
this is a truly magic piece of software that makes it possible to boot
all guests using their own kernel and modules.  See the
[Debian wiki](http://wiki.debian.org/PyGrub) for some details on install
and setup.  Then ignore what you read and do like this:

    # /var/lib/xend/domains/example/config.sxp - Xen domU example.
    # Note the order of partitions in disk=[], the first listed partition
    # is the partition where your /boot (grub) resides. 
    bootloader  = '/usr/bin/pygrub'
    
    builder     = 'linux'
    memory      = '2048'
    root        = '/dev/xvda1 ro'
    
    disk        = [
      'phy:/dev/disk/by-uuid/df3418f5-2fc3-443c-8e64-4395828dc678,xvda1,w',
      'phy:/dev/disk/by-uuid/acecf01e-c4d9-4a7b-b18a-681c69f71173,xvda2,w',
    ]
    
    name        = 'example'
    
    vif         = [ 'bridge=eth1' ]
    dhcp        = "off"
    ip          = "12.34.56.78"
    netmask     = "255.255.255.252"
    gateway     = "12.34.56.77"
    hostname    = "example"
    
    on_poweroff = 'destroy'
    on_reboot   = 'restart'
    on_crash    = 'restart'
    vcpus       = 1
    extra       = 'xencons=tty1'

When installing grub on your guest, don't pay any attention to Grub
complaining about not being able to map `/dev/xvda1` to a BIOS disk.
pyGrub doesn't care about such wordly things as physical disks.  Just
make sure the `/boot` directory's partition (often `/`) is the first
listed in the `disk=[]` array above.
