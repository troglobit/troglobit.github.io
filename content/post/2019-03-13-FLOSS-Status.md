---
title: "FLOSS Progress"
date: 2019-03-13T00:11:32+01:00
subtitle: ""
tags: []
---

This post is an update of what's happened since last I posted about my
many pet projects.  As usual nothing fancy.  If you want to know more I
suggest you find one on GitHub you're interested in.  File an issue, or
better yet, post a pull request to scratch that itch you have deep down:
👉 https://github.com/troglobit

<!--more-->

-----

**editline**, finally saw the addition of event loop support (and some
other cool small features).  Took some time to finalize but it's really
good, [v1.16](https://github.com/troglobit/editline/releases/tag/1.16.0)

**mdnsd**, thanks to [Thom Nichols](https://github.com/thom-nic) I
finished the work I had started three years ago.  It was a great feeling
to get that one out the door!  Honestly, I rarely take the time to look
back at the things I inevitably learn, but this time I did and it's been
very useful professionally.  This one's a little gem, so check it out:
[v0.8](https://github.com/troglobit/mdnsd/releases/tag/v0.8)

[pimd](https://github.com/troglobit/pimd), work continues steadily,
albeit slowly, on this Sisyphus project of mine.  I've been working on
v3.0 for over two years now and there's still a lot of things left to
fix before releasing it.  Regressions for one thing -- consider this an
official plea for help to test it!

[mrouted](https://github.com/troglobit/mrouted), with all the work
that's gone in to pimd over the years I've long felt it was time to give
mrouted its mayby final push.  For one because of its kinship with pimd
(they share a lot of code), but also because it's a *really simple*
multicast router.  The last couple of months I've fixed bugs and added
two critical features: `mroutectl` and proper IGMPv3 support!  It's
almost ready for release, stoked! =)

**sysklogd**, yeah I finally went ahead and adopted this one.  We've
used it at `$work` for ages with a few patches, e.g. buit-in logrotate,
so I published them, cleaned it up and added a few more neat features as
[sysklogd v1.6](https://github.com/troglobit/sysklogd/releases/tag/v1.6)

**mg**, despite there being multiple "portable" projects of OpenBSD Mg,
I still believe mine is the better one.  It uses well known configure,
is completely (!) self-hosting apart from a libc and ncurses (termcap).
[Mg v3.2](https://github.com/troglobit/mg/releases/tag/v3.2)

**libConfuse**, saw only small CVE fix release issued in August, usual
download place at GitHub: [libConfuse
v3.2.2](https://github.com/martinh/libconfuse/releases/tag/v3.2.2)

[sntpd](https://github.com/troglobit/sntpd), the continuation of Larry
Doolittle's ntpclient.  The changes I'd made (oh so long ago, almost 10
years) had started leaking out, and one Linux distribution had actually
packaged my version.  With the obvious consequence that users of said
distro were upset ... with me.  Rather than try to explain the
differences for the Nth time, I finally did what I should have done ages
ago and simply renamed my fork.  Of course there will be a day when
someone will be upset with that too and maybe I'll have more time on my
hands by then, or I can haunt them due to my being dead!
Moahahahahahahaaaa!

-----

There's more, but I'm sure everyone's lost interest by now, so good
night and remember to floss before going to bed! :)

