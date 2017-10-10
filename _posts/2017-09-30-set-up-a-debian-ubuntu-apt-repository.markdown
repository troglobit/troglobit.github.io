---
layout: post
title: "Set up a Debian/Ubuntu APT Repository"
date: 2017-09-30 12:39:43 +0200
comments: true
categories:
---

Quick note, mostly to self, on how to set up a Debian/Ubuntu APT
repository with GPG signing, http://deb.troglobit.com

At first I tried to use [Bas Wijnen's mini-dinstall howto][1], but never
managed to get it working.  Probably due to problems with GPG.  Then I
went down the tried and true path of using [reprepro][2].

I've tried to document my steps here, but I've very likely missed a few
steps that a beginner admin may run into. YMMV B-)

<!-- more -->


### Install

First set up the web server and its directories.  I obviously use
[Merecat httpd][3], but you can use just about any web server:

    $ sudo mkdir /srv/deb
    $ sudo mkdir /var/www/deb.troglobit.com
    $ sudo echo "/srv/deb  /var/www/deb.troglobit.com  none  defaults,bind  0 0" >>/etc/fstab

Then install reprepro

    $ sudo apt install reprepro

The man page and the documentation in `/usr/share/doc/reprepro/` is
quite good, so I recommend at least skimming through it before you
continue.


### Setup

The following steps create the necessary directories and configuration
files:

    $ sudo mkdir -p /srv/deb/{debian,ubuntu}/{conf,dists,incoming,indices,logs,pool,project,tmp}
    $ cd /srv/deb
    $ sudo chown -R `whoami` .

I've created a set of directories for both Debian and Ubuntu, below I
will continue with Debian only.  Notice the use of Debian code names
which are different in the Ubuntu world.

    $ cd debian/conf
	$ mg distributions

Notice the `Suite` and `Codename` for each entry in the `distributions`
file below.  These are the official names and their current release
state in Debian.  Below we will use `reprepro createsymlinks` to
automatically create synmlinks based on this file.

```conf
Origin: Troglobit Software
Label: deb.troglobit.com
Suite: oldstable
Codename: jessie
Architectures: amd64
Components: main
Description: Unofficial Debian/Ubuntu Packages maintained by Joachim Nilsson
SignWith: 4B8786A6

Origin: Troglobit Software
Label: deb.troglobit.com
Suite: stable
Codename: stretch
Architectures: amd64
Components: main
Description: Unofficial Debian/Ubuntu Packages maintained by Joachim Nilsson
SignWith: 4B8786A6

Origin: Troglobit Software
Label: deb.troglobit.com
Suite: unstable
Codename: sid
Architectures: amd64
Components: main
Description: Unofficial Debian/Ubuntu Packages maintained by Joachim Nilsson
SignWith: 4B8786A6
```

Next up is `conf/incoming`:

```
Name: default
IncomingDir: incoming
TempDir: tmp
Allow: oldstable>jessie stable>stretch unstable>sid
Cleanup: on_deny on_error
```

We can now create the initial structure, symbolic links and initial
Packages files:

    $ cd /srv/deb/debian
    $ reprepro -Vb . createsymlinks
    $ reprepro -Vb . export


### Signing

To be able to actually sign `.deb` files you need to create (and also
publish the public part of) a GPG signing key.  Answer the questions to
`--gen-key` below, select type 4 RSA key for signing.  Make sure to use
a 4096 bit key.  I set it up *without a passphrase* for convenince¹ and
then upload the resulting public key:

    $ gpg --gen-key
	gpg: key 44D7FA0A marked as ultimately trusted
	public and secret key created and signed.
	$ gpg --send-keys 4B8786A6
	gpg: sending key 4B8786A6 to hkp server keys.gnupg.net

Add the fingerprint to the `SignWith:` in the `conf/distributions` file.


### archive-keyring.deb

The public part of the GPG key also needs to be published as a regular
`.deb` package.  Clone my [GIT repo](https://github.com/troglobit/deb)
and change the `active/archive.asc` key:

    $ git clone https://github.com/troglobit/deb.git
    $ cd deb/
    $ gpg --export archive@troglobit.com >active/archive.gpg

Then edit the files in the `debian/` sub-directory to match your repo
and build the package:

    $ dpkg-buildpackage

Done.


### Developer

It's now time for the developer(s) to upload the(ir) package(s) to the
`/srv/deb/debian/incoming/` directory.

    $ scp troglobit-archive-keyring_* server:/srv/deb/debian/incoming/

Then run `mini-dinstall` in batch mode from the command line, or start
it in daemon mode:

    $ cd /srv/deb/debian
    $ reprepro -Vb . processincoming default


### Users

Add a dedicated `.sources` file to the `/etc/sources.list.d/` directory,
e.g. `troglobit.sources`:

    Types: deb
    URIs: http://deb.troglobit.com/debian
    Suites: unstable
    Architectures: amd64
    Components: main
    Signed-By: /usr/share/keyrings/troglobit-archive-keyring.gpg

Run an `sudo apt update` and remember to add the GPG key:

    sudo apt-key adv --recv 4B8786A6
    apt install --allow-unauthenticated troglobit-archive-keyring

There.  Packages can now be installed.


### Footnotes

¹ To remove a GPG passphrase from a key, use `gpg --edit-key HASH`, and
then `passwd` at the `gpg>` prompt.  Simply press enter when prompted
for the new passphrase.


[1]: https://debian-administration.org/article/717/Setting_up_a_personal_secure_apt_repository
[2]: https://debian-administration.org/article/286/Setting_up_your_own_APT_repository_with_upload_support
[3]: https://github.com/troglobit/merecat

<!--
  -- Local Variables:
  -- mode: markdown
  -- End:
  -->
