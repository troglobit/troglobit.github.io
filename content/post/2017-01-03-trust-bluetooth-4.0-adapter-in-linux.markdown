---
categories: null
comments: true
date: 2017-01-03T17:03:05Z
title: Trust Bluetooth 4.0 Adapter in Linux
url: /2017/01/03/trust-bluetooth-4.0-adapter-in-linux/
aliases: /blog/2017/01/03/trust-bluetooth-4.0-adapter-in-linux/
---

This is a quick writeup of how to get the Trust Bluetooth 4.0 adapter
(dongle) working in Linux, Ubuntu 16.04.

<!--more-->

The Bluetooth adapter in my ThinkPad X1 Carbon has never worked, it was
a heavily used laptop when I purchased it, so it may have been broken
for some time.  I spent some time early on trying to get it to work, but
to no avail unfortunately.

Today I stumbled upon a quite cheap Bluetooth adapter from Trust at
Net-on-Net here in Västerås.  The adapter use a chipset common to many
such small Bluetooth dongles:

    $ lsusb
    ...
    Bus 003 Device 005: ID 0a12:0001 Cambridge Silicon Radio, Ltd Bluetooth Dongle (HID mode)
    ...

The dongle is a bit stubborn since it starts up in HID mode, instead of
HCI mode, by default.  However, thanks to the power open source there
is a way to switch it around without having to resort to using Windows.

Create the file `/etc/udev/rules.d/97-hid2hci.rules` as root and add
the following line:

    $ sudo vim /etc/udev/rules.d/97-hid2hci.rules
    ATTR{idVendor}=="0a12", ATTR{idProduct}=="0001", RUN+="/lib/udev/hid2hci --mode=hci --method=csr2 --devpath=%p"

Simply unplugging and plugging it back in again didn't work for me, so
reboot your laptop/system/raspberry to get it to work.  Having started
up you can check the output of `lsusb` again:

    $ lsusb
    ...
    Bus 003 Device 005: ID 0a12:0001 Cambridge Silicon Radio, Ltd Bluetooth Dongle (HCI mode)
    ...

Then check `hciool` to verify Linux has found the Bluetooth dongle:

    $ hcitool dev
    Devices:
           hci0	00:1A:7D:DA:71:13

Good Luck! :)

<!--
  -- Local Variables:
  -- mode: markdown
  -- End:
  -->
