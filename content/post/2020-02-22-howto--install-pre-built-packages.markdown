---
title: "HowTo: Install pre-built packages"
subtitle: ""
date: 2020-02-22 17:36:45 +0100
tags: []
---

Every now and then people ask me for pre-built packages of software I
maintain.  Up until today I've had to refer them to the cold hard build
instructions for each of my programs.

<!--more-->

However, today I announce <https://deb.troglobit.com>, a repository of
pre-built amd64 packages built for Debian 10 (Buster), which will work
on any Debian derived distribution; Ubuntu, Linux Mint, etc.

To install packages from this repository,

1. add the archive key:

        curl -sS https://deb.troglobit.com/pubkey.gpg | sudo apt-key add -

2. add the repository to your sources.list:

        echo "deb [arch=amd64] https://deb.troglobit.com/debian stable main" \
             | sudo tee /etc/apt/sources.list.d/troglobit.list

3. update the package cache

        sudo apt-get update

To install some packages:

    sudo apt-get install inadyn mg uftpd mcjoin mini-snmpd

Please let me know when there's anything you'd like me to add.
