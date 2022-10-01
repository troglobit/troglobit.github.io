---
title: Remap Prev/Next on ThinkPad
date: 2022-10-01T15:44:00Z
tags:
 - ThinkPad
 - X200
---

I still use my awesome little [ThinkPad X200][1], in fact I now have
more of them and even a few X201's.  They are truly the best machines
I've ever used! :-)

One annoying thing though, when going back to these traditional ThinkPad
keyboards is the prev/next keys next to the arrow keys.  On modern ones
they are PgUp/PgDn and I recently [learned how to][2] elegantly remap
them in X.  Note the difference, below, to the original at ThinkWiki.
In the (new) file `/usr/share/X11/xkb/compat/thnkpad`, add:

    default partial xkb_compatibility "basic" {
        interpret.repeat= True;

        interpret XF86Back {
            action = Redirect(Key=<PGUP>);
        };
        interpret XF86Forward {
            action = Redirect(Key=<PGDN>);
        };
    };

This may be a new file on your system, so you may need to add it to the
list of "augments" to add.  Edit `/usr/share/X11/xkb/compat/complete` so
it looks something like this:

    default xkb_compatibility "complete" {
        include "basic"
        augment "iso9995"
        augment "mousekeys"
        augment "accessx(full)"
        augment "misc"
        augment "xfree86"
        augment "level5"
        augment "caps(caps_lock)"
    };

Restart your X window system, or reboot, et voilà! \o/

[1]: https://www.youtube.com/watch?v=CVG7SW0ouL8&ab_channel=LaptopRetrospective
[2]: https://www.thinkwiki.org/wiki/How_to_get_special_keys_to_work#Redirecting_XF86Back.2FXF86Forward
