---
title: "cgit with merecat httpd"
subtitle: ""
date: 2018-03-30 19:41:26 +0200
tags: []
---

This post details how to set up [cgit][1] with [Merecat httpd][2].  It
begins as a [GitHub issue][3] report by Mr Alok G. Singh, who had run
into problems setting it up.  I'm posting it here for others to see.

<!--more-->

My server has several sub-domains set up, hence I run [Merecat][2] with
the `virtual-host = true` setting and bind mounts in `/var/www/`.
Detailed in the [online documentation][2].

Setting up [cgit][1] requires a bit of linking & symlinking on top of
the bind mounts, at least on Debian/Ubuntu.  The bundled cgit package is
installed into both `/usr/share` and `/usr/lib`, and when Merecat has
chrooted those directories will not be available.  I've bind mounted my
`/usr/share/cgit` to `/var/www/cgit.troglobit.com` and linked the
`cgit.cgi` from `../../lib/cgit/`:

```sh
root@example:/usr/share/cgit# ll
total 1000
drwxr-xr-x   2 root root   4096 Mar 30 11:16 ./
drwxr-xr-x 140 root root   4096 Mar 30 09:39 ../
-rwxr-xr-x   2 root root 986800 Feb 12  2016 cgit.cgi*
-rw-r--r--   1 root root  13312 Feb 12  2016 cgit.css
-rw-r--r--   1 root root   1278 Feb 12  2016 cgit.png
-rw-r--r--   1 root root   1078 Feb 12  2016 favicon.ico
lrwxrwxrwx   1 root root     22 Mar 30 11:16 filters -> ../../lib/cgit/filters/
lrwxrwxrwx   1 root root      8 Mar 30 10:16 index.cgi -> cgit.cgi*
-rw-r--r--   1 root root     47 Feb 12  2016 robots.txt
```

Merecat httpd currently does not support URL rewrite, so I've set up
cgit as follows, `/etc/cgitrc`:

```cfg
# cgit config

css=/cgit.css
logo=/cgit.png

# Prepend this to all URLs
virtual-root=/cgit.cgi/

# Home of all GIT repos
scan-path=/srv/git/

# if you do not want that webcrawler (like google) index your site
robots=noindex, nofollow
```

*Note the leading slash* in the `virtual-root`.  The resulting site can
be viewed here: http://cgit.troglobit.com

[1]: https://git.zx2c4.com/cgit/
[2]: http://merecat.troglobit.com
[3]: https://github.com/troglobit/merecat/issues/3
