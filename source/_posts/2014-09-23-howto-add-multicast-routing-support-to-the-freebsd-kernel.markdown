---
layout: post
title: "HowTo Add Multicast Routing Support to the FreeBSD kernel"
date: 2014-09-23 01:55:19 +0200
comments: true
categories: 
---

This is a very short blog post, mostly intended as a reminder to
myself.  Assumes the `src.txz` was installed previously.  Here goes:

    cd /usr/src
	cd sys/amd64/conf
	cat GENERIC | sed 's/GENERIC$/MULTICAST/' > MULTICAST
	echo 'options	MROUTING		 # Multicast routing' >> MULTICAST
	cd -
	make kernel KERNCONF=MULTICAST
	reboot

That's it.  Remember to make sure your Qemu VM has enough RAM or it
will probably page fault on you.  I use 1,0 GB RAM.
