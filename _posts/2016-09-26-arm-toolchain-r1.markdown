---
layout: post
title: "ARM Toolchain r1"
date: 2016-09-26 22:43:53 +0200
comments: true
categories:
---

First GCC 6.1 based ARM (32-bit) toolchain released on my FTP.  Built
using [crosstool-NG][1] for Ubuntu 16.04 (x86_64) with musl support.

- [arm-unknown-linux-gnueabi-6.1.0-1.tar.xz][2]

The main purpose for my building this is [TroglOS][3], but it is useful
for other purposes as well of course.  To rebuild it on your system, see
the included crosstool.config file and xthe encoded GIT hash.

Next up is a PowerPC (32-bit) toolchain, also with musl.

[1]: https://github.com/crosstool-ng/crosstool-ng
[2]: http://ftp.troglobit.com/pub/Toolchains/arm-unknown-linux-gnueabi-6.1.0-1.tar.xz
[1]: https://github.com/troglobit/troglos

<!--
  -- Local Variables:
  -- mode: markdown
  -- End:
  -->
