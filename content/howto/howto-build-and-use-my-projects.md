---
title: "HowTo: Build & Use My Projects"
author: "Joachim Nilsson"
type: ""
date: 2020-08-14T07:38:09+02:00
subtitle: "#include <stddisclaimer.h>"
image: ""
tags: []
---

Ever so often someone new to Open Source show up on GitHub or in my
inbox.  It usually starts something like this:

- *"Hey, I just downloaded your software, what's next?"*
- *"Your crappy software doesn't work on my system!!!!"*
- *"How can I do that weird-thing that fits my odd use-case?"*

This HowTo is for you.

## 0. Short Version

- Download the *versioned* archive from the project's Releases page
- Extract archive and change to its directory
- `./configure --help`, or just
- `./configure && make && sudo make install`

> Some projects of mine are available in Debian/Ubuntu/FreeBSD, if you
> use that, please report bugs to them and not me.  I have no control
> over what they do.

Cheers  
 /Joachim

<!--more-->

## 1. I don't owe you free support

Just because the software is published for free, and often without any
restrictions[^1] on use, doesn't mean I must provide you with any support.

If you use my software packaged by a 3rd party, e.g., Debian, FreeBSD,
or similar, please report the bug/problem to them first.  They become
first line support when publishing my work.

[^1]: see the file LICENSE or COPYING included with the software.


## 2. Use **released** software, not GIT sources please

There are some important caveats to consider if you're new to Open
Source software:

- Released software is stable software, use versioned tarballs/zip files
- Software in GIT may be missing important files/scripts
- Software in GIT, even on the master branch, may be unstable/unfinished

When I release software on GitHub I first do a "tag", then build the
release from that tag.  This results in artifacts such as an archive in
the form of a tarball, or zipfile, with a *version* in the name.

> GitHub has the bad habit of automatically creating archives for a
> "tag", don't use those.  They typically **miss important files**.

Released archives, in most of my projects, include a `configure` script,
which usually is not available in raw GIT.

### Please note

Every project on GitHub has a *Releases Page*, e.g.

<https://github.com/troglobit/finit/releases>

Unless you want to contribute to the project, use released software!


## 3. Some assembly required

Pun intended, but don't worry I don't use any assembly language in my
software.  It's more like building furniture from IKEA.

As mentioned previously, and for reasons between different operating
systems, as well as to facilitate cross-compilation to embedded targets
(biggest audience by far!), most of my projects include a configure
script.  This script is generated at release time and is *not available*
from GIT sources.

From the project release page, download the *versioned archive* (means
it has a version number in the name and ends in tar/zip) which includes
the configure script, extract the archive, run the `configure` script
and then run `make`.

Other requirements is a C compiler (I'm old so that's still my jam) and
the header files and .a file(s) for the C library in you system.  Some
of my projects also have other requirements, see the README file or the
output from a failing configure script for details.

### Example

    wget https://github.com/troglobit/ssdp-responder/releases/download/v1.7/ssdp-responder-1.7.tar.gz
    tar xf ssdp-responder-1.7.tar.gz
    cd ssdp-responder-1.7/
    ./configure
    make

When that is done you have the built `ssdp-scan` tool and the `ssdpd`
daemon (UNIX service) available in the `src/` directory.  You can now
either run the programs from there, or install them on your system
using:

    make install

This installs them into the default prefix `/usr/local`, which you can
change by supplying a different prefix path to the configure script.
See `./configure --help` for details.
