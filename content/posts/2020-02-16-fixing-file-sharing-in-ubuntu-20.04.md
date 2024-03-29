---
title: "Fixing file sharing in Debian/Ubuntu/Mint"
subtitle: ""
orig-date: 2020-02-16 21:06:59 +0100
date: 2020-07-13 17:37:00 +0100
tags: []
---

Mounting my ReadyNAS from Nautilus stopped working after upgrading to
Ubuntu 20.04.  Turns out there was a change in behavior in Sambva v4.11
that disabled SMB1 protocol by default.  It'll be interesting to see how
this pans out in the rest of the Linux community ... fortunately there's
a workaround!

> **Update:** same on Debian 11 (*bullseye*) and Linux Mint 20 (*Ulyana*)

<!--more-->

The fix, from [AskUbuntu][] and [Reddit][], suggest lowering the min
protocol version.  To do this, open `/etc/samba/smb.conf` with your
favorite editor:

```shell
~$ sudo mg /etc/samba/smb.conf
```

Add the following line to the `[global]` section:

```aconf
client min protocol = NT1
```

If you're using Nautilus (Files, Nemo), you're done.  But if you're
using Samba, e.g. from the command line, restart it to make change take
effect:

```shell
~$ sudo systemctl restart smbd.service
```

That's it.  If it doesn't work, you could try setting an ever lower
protocol version:

```aconf
client min protocol = CORE
```

For me, however, using `NT1` makes everything work.  Including renaming
files and such, which does not work with `CORE`.

[Reddit]: https://www.reddit.com/r/linuxquestions/comments/djvpdn/smb_connection_nautilus_error_debian_bullseye/
[AskUbuntu]: https://askubuntu.com/questions/1229929/cant-acces-nas-anymore-after-upgrading-to-20-04
