---
title: Buildroot Development Environment
date: 2024-08-07 07:06:19 +0100
tags: [ buildroot, embedded ]
---

Here are a few Buildroot tricks I use to develop and test my packages.
This post will likely evolve over the years to come.

For the basics, please see my post [Buildroot Development Checklist][0].
It covers how to use the `check-package` and `test-pkg` tools shipped
with Buildroot.

> **Tip:** if stuck, always check the [Buildroot documentation][1]!

### introduction

I often need to rebuild from scratch to verify fundamental changes to
the structure of my embedded systems.  So I don't have the patience to
wait for big chunks of code to be downloaded and rebuilt from scratch
every time.  Buildroot has two great options for this:

```
BR2_DL_DIR="$(TOPDIR)/dl"
```

and

```
BR2_CCACHE=y
BR2_CCACHE_DIR="$(TOPDIR)/.ccache"
```

The former is the default while the latter is not.

### defconfig

Alright, the base configuration I use to test my packages in Qemu is called
`configs/troglobit_qemu_x86_64_defconfig`:

```
BR2_x86_64=y
BR2_TOOLCHAIN_EXTERNAL=y
BR2_TOOLCHAIN_EXTERNAL_BOOTLIN_X86_64_UCLIBC_BLEEDING_EDGE=y
BR2_CCACHE=y
BR2_CCACHE_DIR="$(TOPDIR)/.ccache"
BR2_TARGET_GENERIC_ISSUE="Buildroot Testing Ground"
BR2_ROOTFS_DEVICE_CREATION_STATIC=y
BR2_SYSTEM_DHCP="eth0"
BR2_TARGET_TZ_INFO=y
BR2_ROOTFS_POST_BUILD_SCRIPT="board/qemu/x86_64/post-build.sh"
BR2_ROOTFS_POST_IMAGE_SCRIPT="board/qemu/post-image.sh"
BR2_ROOTFS_POST_SCRIPT_ARGS="$(BR2_DEFCONFIG)"
BR2_LINUX_KERNEL=y
BR2_LINUX_KERNEL_USE_CUSTOM_CONFIG=y
BR2_LINUX_KERNEL_CUSTOM_CONFIG_FILE="board/qemu/x86_64/linux.config"
BR2_LINUX_KERNEL_NEEDS_HOST_LIBELF=y
BR2_PACKAGE_BUSYBOX_CONFIG="troglobit_busybox.config"
BR2_PACKAGE_BUSYBOX_SHOW_OTHERS=y
BR2_PACKAGE_PYTHON3=y
BR2_PACKAGE_PYTHON_TERMINALTABLES=y
BR2_PACKAGE_LIBXCRYPT=y
BR2_PACKAGE_EXPAT=y
BR2_PACKAGE_LINUX_PAM=y
BR2_PACKAGE_LINUX_PAM_LASTLOG=y
BR2_PACKAGE_OPENSSH=y
BR2_PACKAGE_SYSKLOGD=y
BR2_PACKAGE_SYSKLOGD_LOGGER=y
BR2_TARGET_ROOTFS_EXT2=y
BR2_TARGET_ROOTFS_EXT2_SIZE="384M"
BR2_TARGET_ROOTFS_SQUASHFS=y
# BR2_TARGET_ROOTFS_TAR is not set
```

> **Note:** this includes a custom .config files for both Linux and
> BusyBox that are currently not included in this post.  YMMV

Apply and build from scratch with:

```bash
$ rm -rf output/
$ make troglobit_qemu_x86_64_defconfig
$ make
```

### local.mk

Now, testing the resulting image has become substantially easier in
Buildroot.  They even ship their own qemu start script, provided you
build the whole shebang (with host Qemu etc.), but we don't have the
patience for this so we're using the Qemu packages provided on our
Debian/Ubuntu or Linux Mint installation (not covered here either).

What I've done instead is to employ the `local.mk` trick to add `run:`
build target:

```
#SSDP_RESPONDER_OVERRIDE_SRCDIR = /home/jocke/src/ssdp-responder
#SYSKLOGD_OVERRIDE_SRCDIR       = /home/jocke/src/sysklogd
#FINIT_OVERRIDE_SRCDIR          = /home/jocke/src/finit
#MG_OVERRIDE_SRCDIR             = /home/jocke/src/mg
#MROUTED_OVERRIDE_SRCDIR        = /home/jocke/src/mrouted
#LIBNL_OVERRIDE_SRCDIR          = /home/jocke/src/libnl

run:
	@line=`stty -g`; stty raw;																\
	qemu-system-x86_64 -nographic -M pc -cpu host -enable-kvm								\
			-kernel output/images/bzImage													\
			-drive file=output/images/rootfs.ext2,if=virtio,format=raw						\
			-append "rootwait root=/dev/vda console=tty1 console=ttyS0 net.ifnames=0 quiet" \
			-netdev tap,id=nd0,ifname=qtap0 -device virtio-net-pci,netdev=nd0				\
			-netdev tap,id=nd1,ifname=qtap1 -device virtio-net-pci,netdev=nd1				\
			-nic bridge,br=virbr0,model=virtio; stty $$line
```

> **Remember:** makefiles need leading tabs!

So whenever I want to test any of my packages I run `make menuconfig`
first to select it.  Maybe uncomment one of them in my `local.mk` to
build unreleased/patched code.

Then:

```bash
$ make foo-rebuild all run
```


[0]: /post/2023-06-04-buildroot-development-checklist/
[1]: https://nightly.buildroot.org/
