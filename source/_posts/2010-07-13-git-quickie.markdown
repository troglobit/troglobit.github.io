---
layout: post
title: "Git Quickie"
date: 2010-07-13 20:45:49 +0200
comments: true
categories: 
- git
---

This is a document I intend to maintain for my own purposes.  It serves
as a quick reminder for the steps needed when creating and working with
git repositories.

First setup a bare repository on the server.

    $ git --bare init projectX.git
    Initialized empty Git repository in /pub/git/projectX.git/

Set one-liner description, visible in gitweb.

    $ echo "Secret project, use ROT13 to decode source code" >projectX.git/description

Set up a `post-update` hook so that the server repo is update
automatically when you push to it.

    $ cp ~/public_html/programming/post-update projectX.git/hooks/

Change how the "Owner" field in gitweb is displayed to include an
obfuscated email address.

    $ vi projectX.git/config
    [gitweb]
        owner = "Joachim Nilsson <troglobit()vmlinux!org>"

These steps have become so common to me that I've setup a script to
create server side git repositories.  You can now push to the server,
after having setup remote location on the client.

    $ git remote add origin ssh://login@example.com/pub/git/projextX.git
    $ git push origin master
    $ git push --tags

