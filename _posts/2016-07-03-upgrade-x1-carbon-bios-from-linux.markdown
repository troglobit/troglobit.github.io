---
layout: post
title: "Upgrade X1 Carbon BIOS from Linux"
date: 2016-07-03 09:57:44 +0200
comments: true
categories:
---

This is a very brief writeup of how to upgrade the BIOS on a X1 Carbon
(G1) from Linux.  For more information on this topic there is always the
excellent [ThinkWiki](http://www.thinkwiki.org/wiki/BIOS_Upgrade).

OK, this post is more about creating the bootable USB stick needed, was
too much of a chicken to try [Flashrom](https://www.flashrom.org/Flashrom) ...

<!-- more -->

First, find your serial number, for querying Lenovo support (BIOS
upgrade) pages.

    sudo dmidecode -t system

The serial number is actually the combination of the Product Name and
the Serial Number.  E.g.

    Handle 0x000D, DMI type 1, 27 bytes
    System Information
    	Manufacturer: LENOVO
    	Product Name: 3460CLG
    	Version: ThinkPad X1 Carbon
    	Serial Number: DEXFXX
    	UUID: MMMMMMMM-NNNN-OOOO-PPPP-QQQQQQQQQQQQ
    	Wake-up Type: Power Switch
    	SKU Number: LENOVO_MT_3460
    	Family: ThinkPad X1 Carbon

Which in my case means `3460CLG-DEXFXX` ... actual serial have been
obfuscated, obviously.  Find the [Drivers & Software][Lenovo] URL at
Lenovo (again, real serial number obfuscated in URL).

Navigate to the "BIOS Update Bootable CD", which in my case turned out
to be called `g6uj21us.iso`.  Now we need to convert the ISO to an IMG
file to be able to `dd` it to a USB stick:

    ./geteltorito.pl -o g6uj21us.img g6uj21us.iso
    Booting catalog starts at sector: 20 
    Manufacturer of CD: NERO BURNING ROM
    Image architecture: x86
    Boot media type is: harddisk
    El Torito image starts at sector 27 and has 65536 sector(s) of 512 Bytes
    
    Image has been written to file "g6uj21us.img".

The link to the Perl script is available on the ThinkWiki page, or from
[GitHub](https://github.com/ksergey/thinkpad/blob/master/geteltorito.pl).
Now, insert a USB stick and unmount it manually, not using Ubuntu/Gnome
file manager's eject thingy!

Write to USB, mine was at `/dev/sdb` BUT YOURS MIGHT NOT BE, WATCH OUT!

    sudo dd if=g6uj21us.img of=/dev/sdb
    65536+0 records in
    65536+0 records out
    33554432 bytes (34 MB, 32 MiB) copied, 16,5346 s, 2,0 MB/s

Reboot your laptop and select UEFI First in BIOS startup order, then F12
and select boot from Generic USB mass storage device.  Done.

Follow the instructions in the tool to upgrade your BIOS.

[Lenovo]: http://support.lenovo.com/se/en/products/Laptops-and-netbooks/ThinkPad-X-Series-laptops/ThinkPad-X1-Carbon-Type-34xx/3460/CLG-DEXFXX


<!--
  -- Local Variables:
  -- mode: markdown
  -- End:
  -->
