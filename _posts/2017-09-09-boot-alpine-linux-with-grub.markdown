---
layout: post
title: "Boot Alpine Linux with GRUB"
date: 2017-09-09 21:37:10 +0200
comments: true
categories: alpine
---

It's fairly easy to replace the slightly unfriendly syslinux with grub
(grub2) in Alpine Linux.  I use v3.6 but YMMV.

<!-- more -->

Notice how I use the tool `blkid` to find the Linux root device.  The
*grub* root device, however, needs to be the first partition, which is
a bit confusing.

```sh
alpine:~# apk del syslinux
alpine:~# apk add grub grub-bios
alpine:~# grub-install /dev/sda
alpine:~# blkid 
/dev/sda3: UUID="caff7641-b9d3-435a-8282-fc3b15ae8b3a" TYPE="ext4"
/dev/sda2: UUID="fe051023-0986-425e-af4c-5e34a24d4756" TYPE="swap"
/dev/sda1: UUID="1c6b6010-4dff-4d10-881f-16899bd4c227" TYPE="ext4"
alpine:~# cat <<EOF >> /etc/grub.d/40_custom

menuentry "Alpine Linux" {
	set root=(hd0,1)
	linux /boot/vmlinuz root=UUID=caff7641-b9d3-435a-8282-fc3b15ae8b3a modules=sd-mod,usb-storage4,ext3 quiet
	initrd /boot/initramfs-vanilla
}
EOF
```

Or edit the file with `mg /etc/grub.d/40_custom` to add the above
`menuentry`.

Now create the `grub.cfg` file from the templates in `/etc/grub.d/*`:

```sh
alpine:~# grub-mkconfig -o /boot/grub/grub.cfg
```

<!--
  -- Local Variables:
  -- mode: markdown
  -- End:
  -->
