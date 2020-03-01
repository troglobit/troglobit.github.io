---
title: "Fixing file sharing in Debian/Ubuntu"
subtitle: ""
orig-date: 2020-02-16 21:06:59 +0100
date: 2020-03-01 16:58:00 +0100
tags: []
---

So file sharing with my ReadyNAS file server stopped working after
upgrading to Ubuntu 20.04.  (*Update:* same on Debian 11 (bullseye)
Turns out there was a change in behavior in Sambva v4.11 that disabled
SMB1 protocol by default.  It'll be interesting to see how this panes
out in the rest of the Linux community ...

<!--more-->

Here's the fix, [from
Reddit](https://www.reddit.com/r/linuxquestions/comments/djvpdn/smb_connection_nautilus_error_debian_bullseye/);
open `/etc/samba/smb.conf` using your favorite editor:

```sh
sudo mg /etc/samba/smb.conf
```

Add the following line to the `[global]` section:

```
client min protocol = CORE
```

Then restart Samba to have Nautilus (Files) working again:

```
sudo systemctl restart smbd.service
```

That's it.

