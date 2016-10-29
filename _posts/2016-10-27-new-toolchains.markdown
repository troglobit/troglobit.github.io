---
layout: post
title: "New Toolchains"
date: 2016-10-27 22:22:44 +0200
comments: true
categories:
---

Finally, fresh from the oven, here are the remaining two toolchains I've
promised, based on GCC 6.1 and GLIBC 2.23.  Download from the [FTP][0]:

- [arm-unknown-linux-gnueabi-6.1.0-2.tar.xz][0]
- [powerpc-unknown-linux-gnu-6.1.0-2.tar.xz][0]
- [x86_64-unknown-linux-gnu-6.1.0-2.tar.xz][0]

Unpack into `/usr/local`, and add to your `$PATH`, e.g.

    export PATH=/usr/local/arm-unknown-linux-gnueabi-6.1.0-2/bin:$PATH

The toolchains are built using [crosstool-NG][1] on Ubuntu 16.04 64-bit,
and are primarily intended for myself and users of [TroglOS][2], but are
generic enough to be useful for other purposes as well.

The `.config` for [crosstool-NG][1] can be retrieved using the `$CROSS`
prefix followed by `-ct-ng.config`, e.g.:

    arm-unknown-linux-gnueabi-cg-ng.config

This revision only contains C and C++ cross-toolchains, but the next
revision will likely also include Go.  Enjoy! :-)

[0]: http://ftp.troglobit.com/pub/Toolchains
[1]: https://github.com/crosstool-ng/crosstool-ng
[2]: https://github.com/troglobit/troglos

<!--
  -- Local Variables:
  -- mode: markdown
  -- End:
  -->
