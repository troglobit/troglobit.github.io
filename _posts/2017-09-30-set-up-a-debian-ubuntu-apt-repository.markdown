---
layout: post
title: "Set up a Debian/Ubuntu APT Repository"
date: 2017-09-30 12:39:43 +0200
comments: true
categories:
---

Quick note, mostly to myself, on how to set up a Debian/Ubuntu APT
repository with GPG signing.

I've chosen to use `mini-dinstall` for this purpose, below are the major
points and I've very likely missed a few steps that a beginner admin may
run into. YMMV B-)

This HowTo is based on [Bas Wijnen's excellent howto][1].

<!-- more -->

### Install

First set up the web server and its directories.  I obviously use
Merecat httpd, but this setup can be served using just about any web
server:

    $ sudo mkdir /srv/deb
    $ sudo mkdir /var/www/deb.troglobit.com
    $ sudo echo "/srv/deb  /var/www/deb.troglobit.com  none  defaults,bind  0 0" >>/etc/fstab

Then install mini-dinstall in daemon mode:

    $ sudo apt install mini-dinstall

The man page is quite good, so I recommend at least skimming through it
before continuing.

Now, create the basic archive directories, with symlinks for `stable`,
`unstable`, and `oldstable` if applicable:

    $ sudo cd /srv/deb/
    $ sudo mkdir sid
    $ sudo mkdir stretch
    $ sudo mkdir jessie
    $ sudo ln -s sid unstable
    $ sudo ln -s stretch stable
    $ sudo ln -s jessie oldstable

### Setup

Now we can set up mini-dinstall, it needs an upload/incoming dir and a
config file:

    $ sudo mkdir -p mini-dinstall/incoming
    $ sudo vim .mini-dinstall.conf

Add the following as contents:

	archivedir=/srv/deb
	archive_style=simple-subdir
	architectures=all, amd64
	generate_release=1
	mail_on_success=0
	release_signscript=/srv/deb/sign.sh
	[oldstable]
	release_codename=jessie
	[stable]
	release_codename=stretch
	[unstable]
	release_codename=sid

Notice the signing script, `/src/deb/sign.sh`, remember to add the
executable flag using `chmod`:

	#!/bin/sh
	rm -f Release.gpg
	gpg --output Release.gpg --local-user archive@troglobit.com --detach-sign "$1"

To be able to actually sign `.deb` files you need to create (and also
publish the public part of) a GPG signing key.  Answer the questions to
`--gen-key`, select type 4 RSA key for signing.  Make sure to set it up
*without a passphrase¹* and then upload the resulting public key:

    $ gpg --gen-key
	gpg: key 44D7FA0A marked as ultimately trusted
	public and secret key created and signed.
	$ gpg --send-keys 44D7FA0A
	gpg: sending key 44D7FA0A to hkp server keys.gnupg.net

The public part of this GPG key also needs to be published on the new
`.deb` server, like this:

    $ mkdir troglobit-archive-keyring
    $ cd troglobit-archive-keyring
    $ gpg --export archive@troglobit.com > troglobit-archive-keyring.gpg

Done.

### Developer

It's now time for the developer(s) to upload the(ir) package(s) to the
`/srv/deb/mini-dinstall/incoming/` directory.  Run `mini-dinstall` as
follows from the command line, or start it in daemon mode:

    $ mini-dinstall --config /srv/deb/.mini-dinstall.conf --batch


### Users

Add the `deb` line to `/etc/sources.list` or a dedicated file in the
`/etc/sources.list.d/` directory, e.g.:

    deb http://deb.troglobit.com sid/all/'

Run an `sudo apt update` and remember to add the GPG key:

    apt install --allow-unauthenticated troglobit-archive-keyring

There.  Packages can now be installed.

_____

¹ To remove a GPG passphrase from a key, use `gpg --edit-key HASH`, and
then `passwd` at the `gpg>` prompt.  Simply press enter when prompted
for the new passphrase.


[1]: https://debian-administration.org/article/717/Setting_up_a_personal_secure_apt_repository

<!--
  -- Local Variables:
  -- mode: markdown
  -- End:
  -->
