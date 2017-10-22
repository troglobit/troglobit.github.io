---
categories:
- unix
- multicast
- docker
- containers
comments: true
date: 2016-03-07T01:37:04Z
title: Testing multicast with Docker
url: /2016/03/07/testing-multicast-with-docker/
---

Recently [issue #70](https://github.com/troglobit/pimd/issues/70) was
reported to [pimd](https://github.com/troglobit/pimd/).  That number of
issues reported is cool in itself, but this was a question about
[Docker](https://www.docker.com) and `pimd`.

Up until that point I had only read about this new fad, and played
around with it a bit [at work](http://www.westermo.com) for use as a
stable build environment for cross-compiling.  I had no idea people
would want to use a Docker container as a multicast sink.  Basically I
was baffled.

The reporter used a Java based tool but simply couldn't get things to
work properly with `pimd` running on the host:

                    eth0
     MC sender ---> [ Server host ]    <--- router running pimd
                           |
                   ________|________
                  /     docker0     \   <--- bridge    ______
                 /         |         \                |      |   <--- MC receiver
      __________/          |          \_______________|______|_____
     \                     |                            /         /
      \                     `------------------>-------'         /
       \________________________________________________________/
          Container ship

<!--more-->

We tried several approaches, but nothing seemed to help.  This became a
bit of blocker for the `pimd` v2.3.2 release and I admittedly lost a bit
of sleep over this.  So finally this weekend, I sat down and whipped my
old [mcjoin](https://github.com/troglobit/mcjoin/) tool up into shape.
I've relied on it for years, but it couldn't send or receive packets,
until now.

Running docker v1.5 in Ubuntu 15.10 I ran this, with `pimd` on the host
and `mcjoin` as a multicast sink for 250 groups in a container:

    cd ~/Troglobit/mcjoin
    docker run -t -i -u `id -u`:`id -g` -v $HOME:$HOME -w $PWD troglobit/toolchain:latest ./mcjoin 225.1.2.3+250
    ^C
    Received total: 2500 packets

The `pimd` and the multicast sender runs on my host, which should not
matter since Linux still has to route the traffic to the `docker0`
interface.  Also, without setting the TTL to 2 (or greater) the
container receives no traffic at all.  Here's what I run in another
terminal on my host:

    ./mcjoin -s -t 2 -c 10 225.1.2.3+250

Although `pimd` is a little slow to register and install the forwarding
rules in the kernel, it sure enough worked on the first attempt! \o/

This is my first real application level experience with Docker, but it
is sure not the last.  Docker is a truly revolutionary new tool!
