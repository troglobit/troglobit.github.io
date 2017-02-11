---
layout: post
title: "Running ikiwiki in Merecat httpd"
date: 2017-02-11 16:03:39 +0100
comments: true
categories:
---

This is a HowTo for setting up [ikiwiki](http://ikiwiki.info/) with
[Merecat httpd](http://merecat.troglobit.com).

First install ikiwiki

    sudo apt install ikiwiki libcgi-session-perl libcgi-formbuilder-perl

Follow the steps to setup a new Wiki or Blog.  In this example we set up a
wiki in our `~/public_html`:

    ikiwiki --setup /etc/ikiwiki/auto.setup
    ...
    Successfully set up wiki:
    url:         http://localhost/~jocke/wiki
    srcdir:      ~/wiki
    destdir:     ~/public_html/wiki
    repository:  ~/wiki.git
    
    To modify settings, edit ~/home.setup and then run:
        ikiwiki --setup ~/home.setup

By default Merecat has per-user `~/public_html` support disabled, this
is for safety purposes.  To build from source, here from GIT, use:

    git clone https://github.com/troglobit/merecat
    ./autogen.sh
    ./configure --enable-public-html
    make
    sudo make install

Now, to start playing with Ikiwiki, simply start the server:

    merecat -c '**.cgi' -p 8080 /usr/lib/w3m

... and open http://localhost:8080/~jocke/wiki/ in your browser :smiley:

**NOTE:** Although Merecat httpd is a fork of
[thttpd](http://acme.com/software/thttpd/).  Compared to its forefather
Mercat is fully capable of running ikiwiki without any patches.  Problems
with port not being included in `HTTP_HOST` or missing trailing slash in
`PATH_INFO`, have all been fixed.

Cheers!

<!--
  -- Local Variables:
  -- mode: markdown
  -- End:
  -->
