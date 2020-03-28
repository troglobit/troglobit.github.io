---
title: Fix blinking WiFi LED on X200
date: 2020-03-28T14:16:00Z
---

Recently got my hands on a ThinkPad X200, the last model without the
useless touchpad and with the awesome classic keyboard.  A fine little
machine that can easily be upgraded with an SSD disk and 8 GiB RAM!

I've set it up with [Linx Mint (LMDE 4)][1] which works great! :)

<!--more-->

Only annoying thing is the WiFi LED blinking like crazy.  Very
disturbing when you're working on something. 

Turns out it was quite easy to fix.  Create the following file:

```
# /etc/modprobe.d/iwlwifi.conf
options iwlwifi led_mode=1
```

Reboot et voilà! \o/

[1]: https://www.linuxmint.com/download_lmde.php
