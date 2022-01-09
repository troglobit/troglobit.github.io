---
title: "Dependency handling in Finit"
subtitle: ""
original-date: 2021-05-01 08:43:06 +0100
date: 2022-01-09 18:46:23 +0100
tags: [ finit, init, opensource ]
draft: false
---
[![finit logo](/images/finit3.png#floatright)](https://troglobit.com/finit.html)

This is a blog post about Finit.  Each post is concluded with a video
summarizing the topic.  The impatient reader can scroll down to the
[video](#video).

Most non-trivial systems require dependency tracking between services.
Not only does it help ensure correct operation, it is also an enabler
for starting services in parallel.  Less known, but just as important,
is handling dependencies at system reconfiguration.

<!--more-->

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Let&#39;s boot up. <a href="https://t.co/AKpDXoh3Vh">pic.twitter.com/AKpDXoh3Vh</a></p>&mdash; ＨＡＣＫＥＲＳＢＯＴ (@Hackers_bot) <a href="https://twitter.com/Hackers_bot/status/1388605672024154114?ref_src=twsrc%5Etfw">May 1, 2021</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script> 

Finit handles dependencies between services with a condition subsystem
comprising three major types:

 * pid
 * net
 * usr
 
> For more information about the condition subsystem in Finit, see:
> <https://github.com/troglobit/finit/blob/master/doc/conditions.md>


## PID Conditions

A `<pid/foo>` condition ensures that the service `foo` has started and
created its PID file in `/var/run/foo.pid`.  Finit tracks all files
created below `/var/run` that ends in `.pid`, or `/pid` for any
sub-directory.  If the PID in the file matches a service started by
Finit that service's provided condition is asserted.

> At reconfiguration it is expected that services "touch" their PID
> files to reassert the condition they provide.  See the documentation
> for details and how to handle non-conformant services.

### Example

```
/etc/finit.d/foo.conf

    service manual:yes foo -fooargs -- Foo service
   
/etc/finit.d/bar.conf

    service <pid/foo> bar -barargs -- Bar service
```

Bar service is started only when `foo` is up and ready.


## Net Conditions

A `<net/...>` condition is for various *basic* network conditions, e.g.,

  * `<net/route/default>`
  * `<net/<IFNAME>/exist>`
  * `<net/<IFNAME>/up>`
  * `<net/<IFNAME>/running>`

### Example

```
/etc/finit.d/inadyn.conf

    service <net/route/default> inadyn -ns -- In-a-Dyn DDNS Client
```

In-a-Dyn is started only when the system has a default route.  Conversely,
it is stopped (SIGTERM) when the default route is removed.


### User Conditions

A `<usr/baz>` condition is completely controlled by the user.  These are
*static* (one-shot) conditions that persist across `initctl reload`
calls, but do not survive a reboot.

All User conditions are controlled using the `initctl cond ..` commands:

```
initctl cond set baz   # Set (assert) user-defined condition
initctl cond clr baz   # Clear (deassert) user-defined condition
```

### Example

```
/etc/finit.d/foo.conf

    service <usr/baz> foo -foargs -- Foo service
```

Foo service is not started until the following command is called:

```
initctl cond set baz
```

## Video

Here's a video showing conditions in action, booting Alpine Linux
3.13.  The video shows how `ntpd` depends on `syslogd`, how this works
and how other conditions can be added.  Worth noting is how dependant
processes are stopped if their dependencies are not satisfied.

<script id="asciicast-460832" src="https://asciinema.org/a/460832.js" async></script>  

Join the [discussion on GitHub][1] or #troglobit on Freenode if IRC is
more your thing.

[1]: https://github.com/troglobit/finit/discussions/169
