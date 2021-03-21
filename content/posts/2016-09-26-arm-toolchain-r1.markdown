---
categories:
- C
- embedded
- toolchain
comments: true
date: 2016-09-26T22:43:53Z
title: ARM Toolchain r1
url: /2016/09/26/arm-toolchain-r1/
aliases: /blog/2016/09/26/arm-toolchain-r1/
---

First GCC 6.1 based ARM (32-bit) toolchain released on my FTP.  Built
using [crosstool-NG][1] for Ubuntu 16.04 (x86_64) with GLIBC 2.23.

- [arm-unknown-linux-gnueabi-6.1.0-1.tar.xz][2]

Download, unpack into `/usr/local`, and add to your `$PATH`

    export PATH=/usr/local/arm-unknown-linux-gnueabi-6.1.0-1/bin:$PATH

There's lots of neat stuff included, both a `sysroot` and a `debug-root`
with GDB and `gdbserver` for target.  For details on using it, see the
[excellent docs][3].

The main purpose for my building this is [TroglOS][4], but it is useful
for other purposes as well of course.  To rebuild it on your system, see
the included crosstool.config file and the encoded GIT hash.

Next up is a PowerPC (32-bit) and x86 (64-bit) toolchain, also with GLIBC
2.23.  Then I may venture into the realm of musl libc based toolchains,
possibly the guise of [CobbleOS](https://github.com/cobble-os/) ...


[1]: https://github.com/crosstool-ng/crosstool-ng
[2]: http://ftp.troglobit.com/pub/Toolchains/arm-unknown-linux-gnueabi-6.1.0-1.tar.xz
[3]: https://github.com/crosstool-ng/crosstool-ng/blob/master/docs/5%20-%20Using%20the%20toolchain.txt
[4]: https://github.com/troglobit/troglos

<!--
  -- Local Variables:
  -- mode: markdown
  -- End:
  -->
