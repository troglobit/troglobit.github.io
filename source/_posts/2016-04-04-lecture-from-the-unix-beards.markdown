---
layout: post
title: "Lecture from the UNIX beards"
date: 2016-04-04 00:14:28 +0200
comments: true
categories: 
---

After the `rm -rf /*` disaster that hit me a couple of weeks ago I've
been rebuilding my setup, restoring the few files I've had backed up,
and collecting advice from the elders.

Turns out there are a few tricks that can save your home directory from
accidents like mine.  The first one is rather obvious, but I'm writing
it down anyway:

1. Keep separate accounts.  If possible, use separate accounts (with
   different permissions obviously) for different projects.  I had my
   private life and work life mixed up, so that's a big no-no to begin
   with.
2. Create a file called `-t` in your `$HOME`

The last bit of advice I had heard about earlier, about fifteen years
earlier, but completely forgotten about.  The trick is to create a file
that will be interpreted by `rm` as an unknown option.  For GNU `rm` the
`-t` is a good choice.

Here are two ways of creating such a file:

    cd
    touch ./-t

or, the perhaps easier to remember:

    touch -- -t

Cheers!
