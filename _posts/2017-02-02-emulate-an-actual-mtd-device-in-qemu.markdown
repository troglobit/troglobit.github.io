---
layout: post
title: "Emulate an actual MTD device in Qemu"
date: 2017-02-02 18:52:18 +0100
comments: true
categories:
---

Having worked with Linux for the last 20 years, and embedded for more
than ten of them, I've become quite a fan of vitalization in general and
Qemu in particular!

Qemu is a fantastic little tool to verify an embedded system without
having to deal with the problems of actual HW until you really have to.
Don't get me wrong, HW excites me like any other nerd, but if the HW is
new and shaky it can be quite a pain to develop higher level functions.

My holy grail is to have a 100% complete and accurate virtualization
target per architecture to test my various software projects on.  That's
why I created [TroglOS](https://github.com/troglobit/troglos).

<!-- more -->

Since most of the systems I've engaged with have had built-in (NOR)
flash, I wanted a similar enough emulated system.  However, many things
like RTC or MTD did not really exist or work the way I wanted.  I can
live without an emulated RTC, but mapping *named* MTD partitions to
different mount points was a showstopper for me.  So one of the small
fixes to Linux I've made is an extension to the mtd2block driver.

The mtd2block driver can take a device, provided by Qemu emulating a
block device from a raw file on the host, and present as an MTD.  The
following patch adds naming of that MTD so the below `/etc/fstab` can
find and mount it just like it would an actual named MTD partition.

Here's the `/etc/fstab` entry:

    mtd:Config	/mnt	jffs2		sync,noatime,nodiratime					0	0

And here's the patch:

``` patch
commit 69f0cbdcfa6a80fbebe206e2bc3e516342da3be8
Author: Joachim Nilsson <troglobit@gmail.com>
Date:   Wed Nov 18 16:38:13 2015 +0100

    mtd2block: Add support for an optional custom MTD label
    
    This patch adds support for an optional MTD label for mtd2block emulated
    MTD devices.  Useful when, e.g. testing device images using Qemu.  The
    following /etc/fstab line in can then be used to mount a file system
    regardless of the actual MTD partition number:
    
        mtd:Config	/mnt	jffs2	noatime,nodiratime	0    0
    
    Kernel command line syntax:
    
        block2mtd.block2mtd=/dev/sda,,Config
    
    The ',,' is the optional erase_size, which like before this patch,
    defaults to PAGE_SIZE if left out.
    
    Signed-off-by: Joachim Nilsson <troglobit@gmail.com>

diff --git a/drivers/mtd/devices/block2mtd.c b/drivers/mtd/devices/block2mtd.c
index b16f3cd..309276c 100644
--- a/drivers/mtd/devices/block2mtd.c
+++ b/drivers/mtd/devices/block2mtd.c
@@ -218,7 +218,7 @@ static void block2mtd_free_device(struct block2mtd_dev *dev)
 
 
 static struct block2mtd_dev *add_device(char *devname, int erase_size,
-		int timeout)
+		char *label, int timeout)
 {
 #ifndef MODULE
 	int i;
@@ -282,7 +282,10 @@ static struct block2mtd_dev *add_device(char *devname, int erase_size,
 
 	/* Setup the MTD structure */
 	/* make the name contain the block device in */
-	name = kasprintf(GFP_KERNEL, "block2mtd: %s", devname);
+	if (!label)
+		name = kasprintf(GFP_KERNEL, "block2mtd: %s", devname);
+	else
+		name = kstrdup(label, GFP_KERNEL);
 	if (!name)
 		goto err_destroy_mutex;
 
@@ -383,8 +386,8 @@ static int block2mtd_setup2(const char *val)
 	/* 80 for device, 12 for erase size, 80 for name, 8 for timeout */
 	char buf[80 + 12 + 80 + 8];
 	char *str = buf;
-	char *token[2];
-	char *name;
+	char *token[3];
+	char *name, *label = NULL;
 	size_t erase_size = PAGE_SIZE;
 	unsigned long timeout = MTD_DEFAULT_TIMEOUT;
 	int i, ret;
@@ -397,7 +400,7 @@ static int block2mtd_setup2(const char *val)
 	strcpy(str, val);
 	kill_final_newline(str);
 
-	for (i = 0; i < 2; i++)
+	for (i = 0; i < 3; i++)
 		token[i] = strsep(&str, ",");
 
 	if (str) {
@@ -416,7 +419,7 @@ static int block2mtd_setup2(const char *val)
 		return 0;
 	}
 
-	if (token[1]) {
+	if (token[1] && strlen(token[1])) {
 		ret = parse_num(&erase_size, token[1]);
 		if (ret) {
 			pr_err("illegal erase size\n");
@@ -424,7 +427,12 @@ static int block2mtd_setup2(const char *val)
 		}
 	}
 
-	add_device(name, erase_size, timeout);
+	if (token[2]) {
+		label = token[2];
+		pr_info("Using custom MTD label '%s' for dev %s\n", label, name);
+	}
+
+	add_device(name, erase_size, label, timeout);
 
 	return 0;
 }
@@ -458,7 +466,7 @@ static int block2mtd_setup(const char *val, struct kernel_param *kp)
 
 
 module_param_call(block2mtd, block2mtd_setup, NULL, NULL, 0200);
-MODULE_PARM_DESC(block2mtd, "Device to use. \"block2mtd=<dev>[,<erasesize>]\"");
+MODULE_PARM_DESC(block2mtd, "Device to use. \"block2mtd=<dev>[,[<erasesize>][,<name>]]\"");
 
 static int __init block2mtd_init(void)
 {
```

<!--
  -- Local Variables:
  -- mode: markdown
  -- End:
  -->
