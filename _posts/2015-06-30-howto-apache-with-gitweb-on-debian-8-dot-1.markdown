---
layout: post
title: "HowTo: Apache with Gitweb on Debian 8.1"
date: 2015-06-30 23:13:48 +0200
comments: true
categories:
- HowTo
- git
- gitweb
- Apache
- Debian
---

I'm posting this in case anyone else gets stuck setting up [Apache][1]
with [Gitweb][2].  Also as a reminder to myself in case I ever need to
set up this all over again.

1. You have all your eggs in one basket (GitHub), and
2. You really like that shiny basket, but
3. You know you're clumsy and usually drop baskets yourself.
4. You are wise (yes you are!) and realize you need another basket, so
5. You set up a server and a domain yourself ...

OK.  Let's start simple, since you are already running the latest Debian
you fire up the command line and install the basics:

    sudo apt-get install gitweb apache2

So it'll complain, you'll clarify your request and soon all required
packages are on your server.  Now what?

Debian has already set up http://localhost/gitweb for you, and if you
have a domain already you should go ahead and edit the master file for
that:

    editor /etc/apache2/sites-available/000-default.conf
    service apache2 reload

If you want to use that for your gitweb needs, then you're done!

<!-- more -->

For me, running http://troglobit.com/gitweb out of the box was not
enough.  I wanted to setup http://git.troglobit.com and also to make
sure to keep really short and pretty URLs to projects I host.  So the
next part of this post is about how to set that up.

My `000-default.conf` is for http://troglobit.com, so I created a new
one for the sub-domain:

    editor /etc/apache2/sites-available/git.conf

... and added the following:

``` apache /etc/apache2/sites-available/git.conf
    <VirtualHost *:80>
        ServerName git.troglobit.com
        DocumentRoot /usr/share/gitweb

        SetEnv GITWEB_CONFIG    /etc/gitweb.conf
        SetEnv GIT_PROJECT_ROOT /srv/git

        <Directory /usr/share/gitweb>
	        Options FollowSymLinks ExecCGI
	        AddHandler cgi-script .cgi

            DirectoryIndex gitweb.cgi

            # Pretty gitweb URLs + pathinfo feature in gitweb.conf
	        RewriteEngine On
	        RewriteCond %{REQUEST_FILENAME} !-f
	        RewriteCond %{REQUEST_FILENAME} !-d
	        RewriteRule ^.* /gitweb.cgi/$0 [L,PT]
        </Directory>

        # Enable git clone over HTTP
        ScriptAliasMatch \
	        "(?x)^/(.*/(HEAD | \
	        info/refs | \
            objects/(info/[^/]+ | \
            [0-9a-f]{2}/[0-9a-f]{38} | \
            pack/pack-[0-9a-f]{40}\.(pack|idx)) | \
            git-(upload|receive)-pack))$" \
            /usr/lib/git-core/git-http-backend/$1
    </VirtualHost>
```

Remember to enable the new sub-domain in Apache and restart the daemon:

    a2ensite git
    service apache2 restart

I'm not using the default Debian GIT repo location, `/var/lib/git`, so I
need to set the project root for both the Apache and the GitWeb config:

``` perl /etc/gitweb.conf
    # See https://github.com/kogakure/gitweb-theme for more help and pointers

    # path to git projects (<project>.git)
    $projectroot = "/srv/git";
    
    @git_base_url_list = ("git://git.troglobit.com", "http://git.troglobit.com");
    
    # directory to use for temp files
    $git_temp = "/tmp";
    
    $site_name = "git.troglobit.com";
    
    # target of the home link on top of all pages
    $home_link = $my_uri || "/";
    
    # html text to include at home page
    #$home_text = "indextext.html";
    
    # file with project list; by default, simply scan the projectroot dir.
    #$projects_list = $projectroot;
    
    # stylesheet to use
    @stylesheets = ("static/gitweb.css");
    
    # javascript code for gitweb
    $javascript = "static/gitweb.js";
    
    # logo to use
    $logo = "static/git-logo.png";
    
    # the 'favicon'
    $favicon = "static/git-favicon.png";
    
    # git-diff-tree(1) options to use for generated patches
    #@diff_opts = ("-M");
    @diff_opts = ();
        
    # Enable PATH_INFO so the server can produce URLs of the
    # form: http://git.hokietux.net/project.git/xxx/xxx
    # This allows for pretty URLs *within* the Git repository,
    # also needs the Apache rewrite rules for full effect.
    $feature{'pathinfo'}{'default'} = [1];
    
    # Neat way of prefixing the top URL listing
    our @extra_breadcrumbs = (
          [ 'Troglobit Software' => 'http://troglobit.com/' ],
        );
    
    # List avatars next to committers
    $feature{'avatar'}{'default'} = ['gravatar'];
    
    # The category name is read from .git/category, in the same manner as .git/description.
    $projects_list_group_categories = 1;
    $project_list_default_category = "misc";
    
    $projects_list_description_width = 80;
    
    # Enable blame, pickaxe search, snapshop, search, and grep
    # support, but still allow individual projects to turn them off.
    # These are features that users can use to interact with your Git trees. They
    # consume some CPU whenever a user uses them, so you can turn them off if you
    # need to.  Note that the 'override' option means that you can override the
    # setting on a per-repository basis.
    $feature{'blame'}{'default'} = [1];
    $feature{'blame'}{'override'} = [1];
    
    $feature{'pickaxe'}{'default'} = [1];
    $feature{'pickaxe'}{'override'} = [1];
    
    $feature{'snapshot'}{'default'} = [1];
    $feature{'snapshot'}{'override'} = [1];
    
    $feature{'search'}{'default'} = [1];
    
    $feature{'grep'}{'default'} = [1];
    $feature{'grep'}{'override'} = [1];
    
    $feature{'highlight'}{'default'} = [1];
```

The impossibly simple Apache config has been ripped from the intro by
[Jonathan McCrohan](http://dereenigne.org/debian/debian-gitweb-server),
the GitWeb config is a mixture of findings on the Internets.  Notice the
breadcrumbs and the grouping settings, very useful.

The magic with the pretty URLs is in both files, all `RewriteRule` lines
in the Apache `.conf` and the `$feature{'pathinfo'}{'default'} = [1];`
setting in `gitweb.conf`.

Also, try out the cool [theme](https://github.com/kogakure/gitweb-theme)
I use, it looks a lot better than the default.

Happy coding!

[1]: http://www.apache.org
[2]: http://git-scm.com/docs/gitweb

