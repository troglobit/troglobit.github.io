---
categories:
- thinkpad
comments: true
date: 2008-07-19T11:09:25Z
title: No Wireless LED on ThinkPad T61
url: /2008/07/19/no-wireless-led-on-thinkpad-t61/
aliases: /blog/2008/07/19/no-wireless-led-on-thinkpad-t61/
---

I've got a ThinkPad T61 with Intel iwl3945 wireless chipset that I
installed fresh with Ubuntu 8.04.  Everything worked flawlessly
out-of-the-box, except for the useless fingerprint scanner and the
wireless LED.  Don't get me wrong, the wireless network worked fine, but
the LED wasn't on.

At first I thought there was something wrong with the LED itself, but a
couple of searches later I found that it was a known limitation of the
2.6.24 kernel included in 8.04.  A driver upgrade was scheduled for
8.04.1 and after a couple of months it was shipped.  Silly me thought
I'd get the upgrade automatically, but it turns out that both the
iwl3945 and the iwl4965, as well as a bunch of other wireless drivers,
are tucked away in [linux-backports-modules-hardy][1], which will not be
installed by default.

So, if you have some wireless issues in Ubuntu 8.04, try installing this
(meta) package (that depends on a suitable version specific kernel) and
see if it helps.

[1]: apt://linux-backports-modules-hardy
