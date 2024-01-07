---
title: Finit custom connectivity check
date: 2023-01-15T18:42:00Z
tags: [ finit, init, opensource ]
---

This post shows how you can extend Finit with your own conditions.  The
example we will use is a simple Internet connectivity checker.  When it
triggers we start BusyBox ntpd which, if started with any other type of
condition (none, default route, etc.) may get stuck trying to resolve
`pool.ntp.org`.

<!--more-->

### Recap

As mentioned in previous posts, the three main condition groups are:

 - pid
 - net
 - usr

Recently another group has been added:

 - sys

It is free to use and populate by users.  Take note, however, of other
plugins and daemons that share the namespace.  For instance, the Finit
`keventd` publish the `<sys/pwr/ac>` condition.

> For details see the Finit documentation:
> <https://github.com/troglobit/finit/blob/master/doc/conditions.md>


### Connectivity Check

There are many methods available, ICMP (ping) is probably the most
common.  But since it is often blocked by firewalls we will use plain
HTTP, of which there are several online servers to choose from:

| **URI**                                           | **Response**             |
|---------------------------------------------------|--------------------------|
| http://network-test.debian.org/nm                 | NetworkManager is online |
| https://fedoraproject.org/static/hotspot.txt      | OK                       |
| http://nmcheck.gnome.org/check_network_status.txt | NetworkManager is online |
| https://www.pkgbuild.com/check_network_status.txt | NetworkManager is online |
| http://connectivity-check.ubuntu.com              |                          |

> The Ubuntu checker returns an empty response, making it very simple to
> use with a basic `curl http://connectivity-check.ubuntu.com`.  Simply
> checking the return value is enough.

### The Script

We don't have curl on our target, only the BusyBox version of wget.  But
that is all we need to create this little script.  I've put mine in the
rootfs with a post-build hook in Buildroot.

```bash
root@anarchy:~# cat /usr/bin/connectivity.sh
#!/bin/sh
# Manages connectivity condition <sys/internet/up>

GEN=/run/finit/cond/reconf
NET=/run/finit/cond/sys/internet
URI=http://connectivity-check.ubuntu.com
SEC=5

isup()
{
	wget -T 1 -qO- $URI 2>/dev/null || return 1
	return 0
}

mkdir -p $NET
while true; do
	if isup; then
		ln -sf $GEN $NET/up
	else
		rm -f $NET/up
	fi
	sleep $SEC
done
```

### The Finit Config

Create a new Finit config for the connectivity script:

    root@anarchy:~# initctl -c edit connectivity

Paste in the following and exit:

    # Manages the <net/internet/up> condition
    service [2345789] connectivity.sh -- Connectivity monitor

Enable it:

    root@anarchy:~# initctl enable connectivity

Now edit the BusyBox `ntpd` config:

    root@anarchy:~# initctl edit ntpd

Change it to the following:

    service [2345] log <!sys/internet/up> ntpd -n -l -I eth0 -p pool.ntp.org -- NTP daemon

This tells Finit to wait for the script's condition and then start ntpd
as a server on eth0, and peer with pool.ntp.org.

Activate all the changes:

    root@anarchy:~# initctl reload

Verify everything has started properly using `initctl`.  Check the
system log messages for the ntpd progress.

```
root@anarchy:~# initctl -p
PID   IDENT            STATUS   RUNLEVELS    DESCRIPTION
==============================================================================
438   connectivity.sh  running  [--2345-789] Connectivity monitor
443   dropbear         running  [--2345-789] Dropbear SSH daemon
455   tty:console      running  [-12345-789] Getty on /dev/console
444   mdnsd            running  [--2345-789] mDNS-SD daemon
445   mini-snmpd       running  [--2345----] Mini snmpd
8379  ntpd             running  [--2345----] NTP daemon
447   smcrouted        running  [--2345----] Static multicast routing daemon
448   ssdpd            running  [--2345----] SSDP Responder
416   sysklogd         running  [S123456789] System log daemon
449   watchdogd        running  [-123456789] System watchdog daemon
450   httpd            running  [--2345----] Web interface
root@anarchy:~# initctl -p cond
PID   IDENT            STATUS  CONDITION (+ ON, ~ FLUX, - OFF)
==============================================================================
8379  ntpd             on      <+sys/internet/up>
```

