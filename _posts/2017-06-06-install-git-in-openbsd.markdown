---
layout: post
title: "GIT, Autoconf and Automake in OpenBSD"
date: 2017-06-06 10:44:55 +0200
comments: true
categories: openbsd
---

Reminder to self:

```
    echo "https://ftp.eu.openbsd.org/pub/OpenBSD/" >/etc/installurl
    pkg_add git autoconf automake libtool
```

Select the latest versions, then add the following to `~/.profile`:

```
    AUTOCONF_VERSION=2.69
    AUTOMAKE_VERSION=1.15
    export AUTOCONF_VERSION AUTOMAKE_VERSION
```

With your selected versions, of course.

<!--
  -- Local Variables:
  -- mode: markdown
  -- End:
  -->
