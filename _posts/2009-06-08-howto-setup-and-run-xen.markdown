---
layout: post
title: "Howto Setup and Run Xen"
date: 2009-06-08 22:05:42 +0100
comments: true
categories: 
- xen
- virtualization
- HowTo
---

This is an extremly brief and quick [Xen][1] tutorial. There are lots of
them already, see your GNU/Linux distribution's wiki, HowtoForge or
other places for a starter guide. This particular HowTo deals with
setting up Xen as easy as possible using Ubuntu 8.04 LTS as host
operating system (dom0 in Xen terms) and Ubuntu 9.04 as guest operating
systems (domU in Xen lingo).

Ubuntu 8.04 LTS comes with a Xen kernel that can run as dom0. This is
quite important, so pay attention to setting that up correctly. For this
I recommend the [HowtoForge Tutorial][2].

**Note:** you will need to upgrade the default Xen 3.2 to 3.3 to be able
to run the latest distributions as guest OS.  See this
[Ubuntu question][3] for more insight.  To get Xen 3.3, simply activate
the Hardy backports repository in your `/etc/apt/sources.list` and, as
usual, pay attention to your `/boot/grub/menu.lst` so that it points out
the Xen 3.3 image in the `defaults=` option.

When done with the basic setup you should install pygrub on your host,
this is a truly magic piece of software that makes it possible to boot
all guests using their own kernel and modules.  See the [Debian wiki][4]
for some details on install and setup.  Then ignore what you read and do
like this:

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

You may need to edit the `update-grub` script before you install grub,
it is located at `/usr/sbin/update-grub`.  Search for `in_xen`, a fairly
long way down.  Make sure it's set to `in_xen=1` before the big
`if[]`-clause that depends on it.  Without this fix the Xen able kernels
in Ubuntu 9.04, and later, are not detected.

Now run `update-grub` and answer yes to the questions. Take good care to
verify that the script actually finds one active (server) kernel and
adds it properly to your `/boot/grub/menu.lst` file.

Another common problem is the lack of a console login.  Usually you
don't need one, but if you'd like to check your domU from within your
dom0 you need to add the file "console" to `/etc/event.d`

    # console - getty
    #
    # This service maintains a getty on the Xen serial console
    # from the point the system is started until it is shut
    # down again.
    
    start on stopped rc2
    start on stopped rc3
    start on stopped rc4
    start on stopped rc5
    
    stop on runlevel 0
    stop on runlevel 1
    stop on runlevel 6
    
    respawn
    exec /sbin/getty 38400 console

That should do the trick!  If it doesn't, then there's plenty of help to
find in the Debian and Ubuntu wikis, see links above. Good luck!

[1]: http://www.xenproject.org/
[2]: http://howtoforge.org/high-performance-xen-on-ubuntu-8.04-amd64
[3]: https://answers.launchpad.net/ubuntu/+question/50326
[4]: http://wiki.debian.org/PyGrub

