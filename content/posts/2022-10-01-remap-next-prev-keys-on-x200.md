---
title: Remap Prev/Next keys on ThinkPad Keyboards
date: 2022-10-01T15:44:00Z
tags:
 - ThinkPad
 - X200
---

I still use my awesome little ThinkPad X200, in fact I now have more of
them and even a few X201's.  They are truly the best machines I've ever
used! :-)

One annoying thing though, when going back to these traditional ThinkPad
keyboards is the prev/next keys next to the arrow keys.  On modern ones
they are PgUp/PgDn and I recently [learned how to][1] elegantly remap
them in X.  In the (new) file `/usr/share/X11/xkb/compat/thnkpad`, add:

    default partial xkb_compatibility "basic" {
        interpret.repreat = True;
        
        interpret XF86Back {
            action = Redirect(Key=<PGUP>);
        };
        interpret XF86Forward {
            action = Redirect(Key=<PGDN>);
        };
    };

Reboot et voilà! \o/

[1]: https://www.thinkwiki.org/wiki/How_to_get_special_keys_to_work#Redirecting_XF86Back.2FXF86Forward
