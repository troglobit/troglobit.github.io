---
title: "Alpine Linux with Finit"
subtitle: ""
date: 2021-02-12 07:24:06 +0100
tags: [ finit, init, opensource ]
---

[Alpine Linux](https://alpinelinux.org/) is an amazing little operating
system.  It's small, boots quick, and easy to use.  The size alone makes
it very attractive to container builders. 

{{% figure src="/images/finit4-screenshot.png" class="center" caption="Alpine Linux started with Finit" %}}

This blog post details how to set Alpine up with the Finit init system,
replacing the default OpenRC.

<!--more-->

### Alpine SDK

After install you need to add some more packages to build Finit from
source.  The example given here builds from GIT sources, which require a
few more steps.  Building from release tarballs is easier and does not
bring the same amount of overhead.

```shell
~$ apk add alpine-sdk autoconf automake libtool linux-headers
```

### Finit Deps

Finit relies on two libraries that used to be built-in but now are their
own separate projects: [libite (-lite)][2] and [libuEv][3].  The former
contains basic frog DNA and the latter a small event loop that basically
is just a wrapper for Linux `epoll()` & C:o.

```shell
~$ git clone https://github.com/troglobit/libite
~$ git clone https://github.com/troglobit/libuev
~$ git clone https://github.com/troglobit/finit
```

Build each of the dependencies first:

```shell
~$ cd libite
~/libite(maaster)$ ./autogen && ./configure && make && sudo make install
...
~/libite(maaster)$ cd ../libuev
~/libuev(maaster)$ ./autogen && ./configure && make && sudo make install
...
```

> **Note:** since Alpine Linux 3.15 `doas` replaces sudo.  See the
> following blog post for details, in short `alias sudo=doas`:
> <https://www.perrotta.dev/2022/02/doas-bridging-the-sudo-gap/>

### Finit

Now lets build Finit for Alpine!

```shell
~/libuev(maaster)$ cd ../finit
~/finit(maaster)$ ./autogen
~/finit(maaster)$ ./contrib/alpine/build.sh
...
*** Done
```
	
Time to wake up!  We're now at the critical part where we can potentially
screw things up -- the installation.

> Please, read the output from the install script carefully!

```shell
~/finit(maaster)$ ./contrib/alpine/install.sh
```

The first question is easy, do you really want to install Finit?  Yes.

For the second question, however, *I recommend answering NO* first and
manually set `init=` in the boot loader, just to make sure everything
works OK.  You can re-run `install.sh` later to repeat the process.

```
*** Install Finit as the system default IOnit (y/N)? N
*** Done
```

### Testing

Reboot your system and be quick to press the Tab key to enter the Alpine
boot loader menu.  Press Tab again to edit the kernel command line, we
want to:

  1. Remove the 'quiet' option
  2. Append the 'init=/sbin/finit' option
  
The second of the two is the interesting part, that's what tells the
kernel to not use its built-in heuristics to select the first user space
program to start, but instead start Finit.  We remove the quiet option
just to get some more debug info from the kernel and a boot progress
from Finit itself.

Press Enter to boot.

If everything worked you should see the bootup and be presented with the
login prompt, as shown below.


If you like the setup, you can go ahead and run the install script again
and this time answer Yes to the last question.

Use the `initctl` companion tool to Finit to start/stop, activate and 
reload new configurations.

Good luck and have fun! :)

