---
categories: null
comments: true
date: 2016-08-13T15:59:36Z
title: Fake RAID Adventures
url: /2016/08/13/fake-raid-adventures/
---

The other day I got my geeky hands on two old SuperMicro X8STI-F 1U
servers.  I plan to use them as build and embedded target emulation
servers for my [open source projects](https://github.com/troglobit)
as well as Minecraft server for my kids :)

<img src="http://eitc.in/yahoo_site_admin/assets/docs/Supermicro_SuperServer_SC512L_small.324203750.JPG">

<!--more-->

During the install of Ubuntu Server 16.04 I realized I wanted try to use
the Intel PCH "Fake Raid" on the two 1 TB disks, RAID-1.  The support
Intel has in Linux and `mdadm` for their southbridge RAID controller is
really excellent!

I'm not so sure though about the level of support that Debian/Ubuntu and
systemd have for RAID controllers though.  I've run into problems with
how to shutdown, reboot, and power-off the servers with the RAID.

So far I've made out that it is possible to power-off the servers with
the power button, but only straight after boot!  To make matters worse
it doesn't seem that Ubuntu sets the IMSM controller in "idle" state
when rebooting, so unmount (remount ro) fails.  I *think* this is the
recommended way:

    echo idle > /sys/block/md127/md/sync_action

... for all the `md` devices found by globbing `/sys/block/md*` ...

And then wait for state to become idle before unmounting and remounting
`/` read-only, as you have to do, but that doesn't seem to be done, so
when the system is booted again it always comes up in a degraded state
:-/

Anyhow, this little adventure taught me a few tricks worth remembering.


### Show status of Intel Container

This command lists member arrays and disks used in the container:

    user@example:~$ sudo mdadm -D /dev/md/imsm0

This command details RAID (firmware) capabilities:

    user@example:~$ sudo mdadm --detail-platform
           Platform : Intel(R) Matrix Storage Manager
            Version : 8.9.1.1002
        RAID Levels : raid0 raid1 raid10 raid5
        Chunk Sizes : 4k 8k 16k 32k 64k 128k
        2TB volumes : supported
          2TB disks : not supported
          Max Disks : 6
        Max Volumes : 2 per array, 4 per controller
     I/O Controller : /sys/devices/pci0000:00/0000:00:1f.2 (SATA)


### Remove a disk from a container

Say disk `/dev/sda` is unhealthy in any of the member volumes, fail it
in all volumes, remove it, and zero out the superblock to re-add:

    user@example:~$ sudo mdadm /dev/md/Boot -f /dev/sda
    user@example:~$ sudo mdadm /dev/md/Root -f /dev/sda
    user@example:~$ sudo mdadm /dev/md/imsm0 -r /dev/sda
	user@example:~$ sudo mdadm --zero-superblock --force /dev/sda
    user@example:~$ sudo mdadm /dev/md/imsm0 -a /dev/sda

Here `Root` and `Boot` are my member volumes in the Intel PCH `imsm0`
container.  I use a small `Boot` partition for `/boot`, first partition
on the array, to be able to boot using GRUB from it.

If the above doesn't work for you (it didn't for me), try zeroing out
the complete device, which naturally will take a while, try smaller
blocksize (bs) if the following does not work:

    user@example:~$ sudo dd if=/dev/zero of=/dev/sda bs=100M

Then reboot and tell the Intel firmware (Ctrl-I maybe) at POST to add
the "new" disk to the array, or rather to the *container* to be correct.
When booting up the system again, Linux starts a background rebuild.


### Check satus of RAID

Check progress of the rebuild with:

    user@example:~$ cat /proc/mdstat
    Personalities : [raid1] [linear] [multipath] [raid0] [raid6] [raid5] [raid4] [raid10] 
    md125 : active raid1 sda[1] sdb[0]
          2097152 blocks super external:/md127/0 [2/1] [_U]
          	resync=DELAYED
          
    md126 : active raid1 sda[1] sdb[0]
          974660608 blocks super external:/md127/1 [2/1] [_U]
      [===>.................]  recovery = 17.5% (171137152/974660740) finish=103.4min speed=129396K/sec
              
    md127 : inactive sda[1](S) sdb[0](S)
          5024 blocks super external:imsm
               
    unused devices: <none>

Which is very useful with the `watch` command!

Good Luck!

<!--
  -- Local Variables:
  -- mode: markdown
  -- End:
  -->
