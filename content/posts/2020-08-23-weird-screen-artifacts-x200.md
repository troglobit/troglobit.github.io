---
title: Weird Screen Artifacts on X200
date: 2020-08-23T14:02:00Z
tags:
 - ThinkPad
 - X200
---

I blogged earlier about the awesome little ThinkPad X200 I found and its
blinking WiFi LED.  Briefly I mentioned an odd issue with X/Wayland on
LMDE 4.  This turned out to be a HW bug that can only be worked around
by disabling hardware acceleration for virtualization:

https://forums.lenovo.com/t5/Windows-8-1-8-7-Vista-and-XP-Discussions/Bizarre-screen-artifacts-on-R400-Integrated-Graphics-running-Win-7-RTM/m-p/153980?page=1#199768

The post says it should be sufficient to *"Disable Virtualization
Technology for Directed-IO (VT-d)"*, but that didn't work for me,
and I wanted to keep the 8 GiB of RAM I managed to fit into it.
What *did work*, however, was to disable all the virtualization
options.
