---
lastmod: 2024-09-11T11:38:04-01:00
title: A life without sudo
date: 2016-12-11T20:26:12Z
url: /2016/12/11/a-life-without-sudo/
aliases: /blog/2016/12/11/a-life-without-sudo/
categories:
- sudo
- sandwich
- linux
- capabilities
comments: true
---

{{% figure src="/images/sandwich.png" link="https://xkcd.com/149/"
     alt="Make me a sandwich.  What? Make it yourself."
	 caption="<b>Fig 1.</b> Classic XKCD cartoon :-)"
     width="260" class="right-floated" %}}

> *Please see the [latest update](#update-sep-15-2024), below!*

Ever since my first stumbling steps with Linux back in '96, I've been
learning about UNIX.  The first obvious lesson was to not use the root
account.  Since then I've been using a combination of `sudo command` and
suid root binaries to get the job done.

For the last ten years, however, I've been meaning to learn about Linux
[capabilities(7)](http://man7.org/linux/man-pages/man7/capabilities.7.html)
and thanks to a [friend and colleague][wkz] of mine I now have :)

What you want is to grant capabilities per user *and* application.  Most
tutorials only tell you how to do one or the other.

First of all you need to figure out what capabilities an application
requires to perform an action.  Let's use `tcpdump` as an example, it
needs raw link access to sniff packets so your user (you), need to be
listed in the system `/etc/security/capability.conf` file:

```
cap_net_raw     joachim
```

Second, you need to set this on the application, so that when `joachim`
wants to run `tcpdump` he is granted the capability:

```bash
$ sudo /sbin/setcap cap_net_raw+ep /usr/sbin/tcpdump
```

Some applications require multiple capabilities, like Qemu when you use
tap networking.  Update `/etc/security/capability.conf`

```
cap_net_raw,cap_net_admin     joachim
```

Place all capabilities on one line, separated with comma.  Then add both
capabilities to qemu, like this:

```bash
$ sudo /sbin/setcap cap_net_raw,cap_net_admin+ep /usr/bin/qemu-system-arm
```

----

### Update Sep 15, 2024

As time has progressed so has Linux distributions.  On Debian derived
distributions like Ubuntu, or my personal favorite, [Linux Mint][mint],
you now have to adjust your PAM setup slightly:

Edit the file `/etc/pam.d/common-auth` and change the `pam_cap.so` line:

```apacheconf
...
# Comment this line
#auth   optional                        pam_cap.so
# Add this line, notice the 'require' keyword
auth    required                        pam_cap.so
...
```

Without it, your inherited capability mask will not be set properly!

### Helper Script

Here's a little script I use to set capabilities.  It used to be called
`wkz-caps.sh`, but I've since refactored it.  You need to run it every
time after upgrading any of the packages on your system that provides
the tools you need capabilities for.

```bash
#!/bin/sh

set_capabilities()
{
    capability="$1"
    shift
    for cmd in "$@"; do
        binary=$(command -v "$cmd" 2>/dev/null) || binary=$cmd
        path=$(realpath "$binary") # ip tool may be a symlink
        if [ -x "$path" ]; then
            sudo setcap "$capability" "$path"
        else
            echo "Warning: $cmd not found or not executable, skipping." >&2
        fi
    done
}

set_capabilities 'cap_net_admin,cap_net_raw,cap_net_bind_service+ep' \
    gdb-multiarch dnsmasq

set_capabilities 'cap_net_admin+ep' \
    ip bridge brctl ifconfig ethtool \
    qemu-system-aarch64 qemu-system-arm qemu-system-ppc \
    qemu-system-ppc64 qemu-system-x86_64 \
    /usr/lib/qemu/qemu-bridge-helper

set_capabilities 'cap_net_raw+ep' \
    tcpdump nemesis hping3 trafgen
```

[wkz]:   https://github.com/wkz/
[mint]:  https://linuxmint.com/
