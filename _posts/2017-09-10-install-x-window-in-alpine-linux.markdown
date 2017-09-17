---
layout: post
title: "Install X-Window in Alpine Linux"
date: 2017-09-10 11:13:39 +0200
comments: true
categories: alpine
---

How to install LXDM + XFCE4 in Alpine Linux 3.6 when testing with
[Finit](https://github.com/troglobit/finit/tree/master/contrib/alpine).

<!-- more -->

The following sets up everything needed to run in KVM or virt-manager.
The QXL driver is probably not needed for other environments, and there
are other icon themes than Tango.

The evdev driver requires the evdev kernel module, and you may also need
to load the mousedev kernel module.

```
setup-alpine
setup-xorg-base
apk add xfce4 lxdm
apk add xf86-video-qxl
apk add xf86-input-keyboard xf86-input-mouse xf86-input-evdev
apk add tango-icon-theme
```

Now, configure X, it may be necessary when running in KVM or virt-manager:

```
X -configure
```

Edit the file `xorg.conf.new` as needed, possibly you need to disable X
automatic features for adding and enabling devices.

```
Section "ServerLayout"
	Identifier     "X.org Configured"
	Screen      0  "Screen0" 0 0
	InputDevice    "Mouse0" "CorePointer"
	InputDevice    "Keyboard0" "CoreKeyboard"
	Option "AutoAddDevices"    "false"          # <-- 
	Option "AutoEnableDevices" "false"          # <--
EndSection
```

There seems to be a bug in the Qemu QXL driver, so make sure to also set
the following:

```
Section "Device"
	Option      "ENABLE_SURFACES"	"false"     # <--
	Identifier  "Card0"
	Driver      "qxl"
	BusID       "PCI:0:2:0"
EndSection
```

Then copy it to its place:

```
cp xorg.conf.new /etc/X11/xorg.conf
```

Now you should be able to use `startx` to test your installation.  If
you want LXDM to work properly you need to modify `/etc/lxdm/lxdm.conf`.
Find a commented row that says `sessions`, add a new one that is:

```
session /usr/bin/startxfce4
```

For more help with LXDM, see https://wiki.archlinux.org/index.php/LXDM

<!--
  -- Local Variables:
  -- mode: markdown
  -- End:
  -->
