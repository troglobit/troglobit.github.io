---
layout: post
title: "Blog Restoration Project now also on GitHub Pages"
date: 2015-01-02 14:12:59 +0100
comments: true
categories:
---

I've had this long-standing issue with backups.  It's deadly boring to
set up and maintain, so I don't do any.  Until today!

Today I moved the sources for my [Octopress blag](http://troglobit.com)
to GitHub, which also prompted me to set up
[a mirror](http://troglobit.github.io) on
[GitHub Pages](http://pages.github.io).  As usual, reading up on the
subject and muster enough motivation took me about three months, whereas
the actual work took about 4h.

<!-- more -->

I now have a concoction of mixed remotes, multiple branches and some
Ruby magic to administer it all with!  Here are some links I used to
get where I am:

* http://octopress.org/docs/setup/
* https://help.github.com/articles/creating-project-pages-manually/
* https://help.github.com/articles/user-organization-and-project-pages/
* http://www.tomordonez.com/blog/2012/06/04/creating-a-github-blog-using-octopress/
* http://miguelcamba.com/blog/2013/04/22/tutorial-create-a-blog-with-octopress-and-host-it-in-github-pages/
* http://octopress.org/docs/deploying/github/

With some added `apt-get intall ruby ruby-bundler` magic in Ubuntu
everything started to look normal -- now I could even do `rake preview`
before deploying!  Only a minor problem I had before getting everything
working was this:

> /usr/local/rvm/gems/ruby-2.1.2/gems/execjs-2.2.0/lib/execjs/runtimes.rb:51:in `autodetect':
> Could not find a JavaScript runtime. See https://github.com/sstephenson/execjs
> for a list of available runtimes. (ExecJS::RuntimeUnavailable)

Turns out I had to install nodejs as well, dunno why.

    sudo apt install nodejs

Since I wanted a mirrored setup I chose to have Octopress setup default
to deploy to GitHub Pages.  Then I simply rsync the whole shebang to my
own server.

All this means I can now finally begin restoring more content from
[my old blog](/blog/2013/02/17/resurrection/) thanks to partial backups
I've found at the [Wayback Machine](https://archive.org/) :-)
