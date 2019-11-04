---
title: "BSD syslogd in Linux"
subtitle: "modern syslog and standards"
date: 2019-11-03 23:04:00 +0100
tags: [ opensource, sysklogd ]
---

Some time ago now I was in a meeting with a customer where I first learned of
the new syslog standard, [RFC5424][].  I say "new" because, despite it being
ten years old, it was new to me.

Three weeks ago I started updating my fork of [sysklogd][] to be [RFC5424][]
compliant.  I ended up ripping out most of the old code and replacing it with
fresh DNA strands from FreeBSD and NetBSD.

The result is a fully RFC compliant `syslogd`, **and** a `libsyslog` with a
replacement `syslog.h` header for sending RFC5424 events from applications!

<https://github.com/troglobit/sysklogd/>

<!--more-->

When I first embarked on this journey I was very surprised to learn that Linux
(GLIBC) did not have an RFC5424 compliant `syslog()` API.  As it turned out,
neither did FreeBSD.  Only NetBSD had implemented a full stack, from C-library
to syslog daemon.

So the `libsyslog` and `syslog.h` replacements included now in sysklogd are
taken from NetBSD, whereas the major parts of the [RFC3164][] and [RFC5424][]
parsing code in syslogd are taken from FreeBSD.


Differences
-----------

So what are the main differences between the two RFCs?  Well, there are in
fact three different standards at play here.  The first is the original BSD
implementation, which was later sort of standardized in [RFC3164][], this
original BSD is what sysklogd was.

One major difference from the original BSD code and RFC3164, I learned, is
that when sending messages to a remote syslog server, there was no timestamp
or hostname included.  So the receiving syslog server had to look up the host
from the source address in the IP header and then also do the timestamping.
In some respects this is good (and understandable): 1) early on many hosts did
not properly set their hostname, and 2) these same hosts often did not even
have the correct time.

The first job of updating `sysklogd` was to update it to properly support
RFC3164 formatting of remote syslog messages:

    <------- HEADER --------> <----------- MSG ------------>
    Aug 24 05:14:15 192.0.2.1 myproc[8710]: Kilroy was here.
	 \                \        \      \      \
      \                \        \      \      `-- What you think is the message
       \                \        \      `-------- Convention for process ID (PID)
	    \				 \	      `-------------- Convetion for process name
		 \                `---------------------- Hostname or IP address
	      `-------------------------------------- Notice lack of year ...

The second part was to add RFC5424 formatting:

    <---------------------- HEADER ------------------------>   <---- MSG ----->
    2003-08-24T05:14:15.000003-07:00 192.0.2.1 myproc 8710 - - Kilroy was here.
     \                                \         \      \    \ \ \
      \                                \         \      \    \ \ `-- Actual message
       \                                \         \      \    \ `--- Structured data
        \                                \         \      \    `---- Message ID (MsgID)
         \                                \         \      `-------- Process ID (ProcID)
          \                                \         `-------------- Process name (App-Name)
           \                                `----------------------- Hostname or IP address
            `------------------------------------------------------- RFC3339 Timestamp
	
Yeah, what about those dashes (`-`)?  I had to fake it to begin with, because
those dashes, if you disregard the difference in the timestamp, is what are
the major differences between the two RFCs.  As you can see from the figures
above, the RFC3164 world was empty and scary, and the RFC5424 world is full of
confusing stuff??

The first dash above is the new MesgID field, which is a free-form string that
a receiver can use for quick and easy regexp filtering.  The second dash is
*structured data* and is essentially name-value pairs in a `[ .. ]` container.
See the RFC for more details and [an example in section
6.3.5](https://tools.ietf.org/html/rfc5424#section-6.3.5)

To distinguish between the two formats RFC5424 added a version field that
comes right after the `<PRI>` field, which is at the very start of each syslog
message.  I.e., RFC3164 starts with `<PRI>` and a space, and RFC5424 starts
with `<PRI>1` and a space, before the timestamp.


NetBSD API
----------

The GLIBC, musl, and uClibc libraries all currently just support RFC3164.  So
there is no way for a Linux application today to add a MsgID or any structured
data to a syslog message ... until now.

`sysklogd` comes with `libsyslog` and a `syslog.h` header replacement that
has the NetBSD `syslogp()` family of APIs:

```c
int  setlogmask  (int maskpri);
int  setlogmask_r(int maskpri, struct syslog_data *data);

void openlog   (const char *ident, int logopt, int facility);
void openlog_r (const char *ident, int logopt, int facility, struct syslog_data *data);

void closelog  (void);
void closelog_r(struct syslog_data *data);

void syslog    (int priority, const char *message, ...);
void vsyslog   (int priority, const char *message, va_list args);

void syslogp   (int priority, const char *msgid, const char *sd, const char *message, ...);
void vsyslogp  (int priority, const char *msgid, const char *sd, const char *message, va_list args);

void syslog_r  (int priority, struct syslog_data *data, const char *message, ...);
void vsyslog_r (int priority, struct syslog_data *data, const char *message, va_list args);

void syslogp_r (int priority, struct syslog_data *data, const char *msgid, const char *sd,
                const char *message, ...);
void vsyslogp_r(int priority, struct syslog_data *data, const char *msgid, const char *sd,
                const char *message, va_list args);
```

Example
-------

To use the new API on Linux we create an example application:

```c
/* example.c - Example of how to use NetBSD syslogp() API */
#include <stdio.h>
#include <syslog/syslog.h>

int main(void)
{
        openlog("example", LOG_PID, LOG_USER);
        syslogp(LOG_NOTICE, "MSGID", NULL, "Kilroy was here.");
        closelog();
}
```

We build and run the application.

```sh
jocke@troglobit:~/tmp $ gcc `pkg-config --libs --static --cflags libsyslog` \
                            -o example example.c
jocke@troglobit:~/tmp $ ./example
```

Provided `syslogd` is set up properly the following would be logged in your
local log file, or sent to a remote syslog server:

    2019-11-03T00:46:34.034324+01:00 troglobit example 1234 MSGID - Kilroy was here.


[RFC3164]:  https://tools.ietf.org/html/rfc3164
[RFC5424]:  https://tools.ietf.org/html/rfc5424
[sysklogd]: https://www.infodrom.org/projects/sysklogd/
