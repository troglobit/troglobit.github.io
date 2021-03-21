---
title: "sudo tricks"
subtitle: ""
date: 2020-06-07 12:25:52 +0200
categories:
- sudo
- linux
---

> "My name is Joachim and I'm a `sudo su -` user."

Before you read any further, please read [A life without sudo](/2016/12/11/a-life-without-sudo/).

<!--more-->

As an avid user of sudo, despite enjoying the wonders of capabilities, I
appreciate all the extra bells and whistles it comes with.  Here's a couple
of my favorites.

Remeber to edit `/etc/sudoers` with the proper command, which validates the
input to prevent you from locking yourself out:

    sudo visudo

## 1. Remember auth. in other terminal windows

When using `sudo` in one terminal and then moving to another, you have to
type in your password again.  To avoid that and reuse your "ticket", add
the following line after existing `Defaults`:

    Defaults	!tty_tickets

## 2. Visual feedback in sudo prompt

I'm very clumsy with my fingers, in particular early in the morning, add
`pwfeedback` to the line:

    Defaults	env_reset
 
Like this:

    Defaults	env_reset,pwfeedback

Now you can see a `*` for each character you input.  Now you don't have
to Ctrl-U and retype everything.

