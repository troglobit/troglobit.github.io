---
categories:
- openbsd
comments: true
date: 2017-06-06T10:44:55Z
title: GIT, Autoconf and Automake in OpenBSD
url: /2017/06/06/install-git-in-openbsd/
aliases: /blog/2017/06/06/install-git-in-openbsd/
---

Reminder to self:

```sh
echo "https://ftp.eu.openbsd.org/pub/OpenBSD/" >/etc/installurl
pkg_add git autoconf automake libtool
```

Select the latest versions, then add the following to `~/.profile`:

```sh
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
