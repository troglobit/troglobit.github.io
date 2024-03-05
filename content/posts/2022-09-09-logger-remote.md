---
title: "Logging to remote syslog servers"
date: 2022-09-09 19:36:44 +0100
tags: [ opensource, sysklogd ]
---

The [logger(1)][1] tool in your system, `/usr/bin/logger`, standardized in
[IEEE Std 1003.2][2] ("POSIX.2"), has many different implementations.  For
Linux users the most common one is part of the [util-linux][3] package.

I've always considered this one of those weird Linux:isms.  The logger tool is
closely tied to the system log daemon, so in the [sysklogd project][4] we ship
our own implementation of logger.  Our implementation is derived from the
[Finit project's][5] logit tool.

The [sysklogd logger][1] is paired feature wise with [sysklogd][4], supporting
both RFC3164 (BSD) and RFC5424 (new) style logging.  It also supports logging
to file, including log rotation, *and* in the latest sysklogd release, v2.4.4,
logger supports logging to a remote host:

```shell
~$ logger -h 192.168.1.1 "Kilroy was here"
```

Command line options are modeled on the corresponding FreeBSD flags, where
applicable, not util-linux.  See the [FreeBSD man page][6] for comparison.

[1]: https://man.troglobit.com/man1/logger.1.htm
[2]: https://pubs.opengroup.org/onlinepubs/9699919799/utilities/logger.html
[3]: https://www.kernel.org/pub/linux/utils/util-linux/
[4]: https://github.com/troglobit/sysklogd/
[5]: https://github.com/troglobit/finit/
[6]: https://www.freebsd.org/cgi/man.cgi?query=logger&apropos=0&sektion=0&manpath=FreeBSD+13.1-RELEASE+and+Ports&arch=default&format=html
