---
categories:
- Debian
- Ubuntu
- iTunes
- HowTo
comments: true
date: 2010-12-31T20:26:37Z
title: 'HowTo: Use Ubuntu to Serve Music to iTunes'
url: /2010/12/31/howto-setup-debian-slash-ubuntu-to-serve-music-to-itunes/
---

OK, so we finally got an iPad.  The effective marketing droids of Apple
are doing a good job, even the Linux zealots are starting to use their
products :-)

So, how to serve the immense music collection of our family to the iPad?
Well, it seems the magic integration with iTunes is not enough (yet), so
you need a [Simple Daap Client][1] app on your iPad to get the most out
of this HowTo. I hope DAAP sharing for iPad will be added soon, that
would be really cool!

I used the [Firefly Media Server][2], which in Debian/Ubuntu is known as
[mt-daapd][3].  It needs som minor setting up to play nice with the
Avahi multicast DNS (mDNS) services that we will use to let iTunes
discover our new service.

First go ahead and install mt-daapd

    sudo apt-get install mt-daapd

Then make sure to edit the file `/etc/default/mt-daapd` file, it usually
does not exist in the default setup, so create it and add the following
contents to disable the built-in mDNS server:

    # -m Disables the built-in mDNS server, useful if you already run Avahi
    DAEMON_OPTS="-m"

The default configuration of must be updated with the location of your
music library.  On my system it is `/pub/Music`, so I edit the file
`/etc/mt-daapd.conf` and change:

    # Location of the mp3 files to share.  Note that because the
    # files are stored in the database by inode, these must be
    # in the same physical filesystem.
    mp3_dir = /pub/Music

If you like, you can change the `admin_pw` (mt-daapd) and default port
to something other than the defaults.  After changing the settings you
need to restart the service:

    sudo service mt-daapd restart

Now enter `localhost:3689` in your browser window, leave the username
field empty and set password to mt-daapd.  Unless you changed any of the
defaults above.

Time for multicast!  The Avahi service on GNU/Linux systems is what
Bonjour/Rendez-Vous is to Apple systems.  It provides seamless
interaction between clients and servers on a LAN.  My HP printer, for
instance, pops up automatically in Windows, OS X and Linux these days
because these operating systems listen in on network mDNS servers that
broadcast (or rather multicast) available services.  Very neat.

However, as usual there are a few snags you might need to work around to
get everything to work.  Thanks to my ISP (Telia, Sweden), I need to tell
Avahi the following nasty things in `/etc/default/avahi-daemon`

    # 1 = Try to detect unicast dns servers that serve .local and disable avahi in
    # that case, 0 = Don't try to detect .local unicast dns servers, can cause
    # troubles on misconfigured networks
    AVAHI_DAEMON_DETECT_LOCAL=0

And also edit the file `/etc/avahi/avahi-daemon.conf`, the following is
not the entire contents of the file, only the sections I've changed:

    [server]
    # Comment out any current/previous work arounds
    #domain-name=local
    #domain-name=.alocal
    disallow-other-stacks=yes
    
    [publish]
    publish-workstation=no
    publish-domain=no

Also, in the directory `/etc/avahi/services/` we need to add an entry for
DAAP.  Create the file `/etc/avahi/services/daap.service`:

    <!DOCTYPE service-group SYSTEM "avahi-service.dtd">
    <!-- iTunes DAAP, music streaming, for mt-daapd or Firefly Media Server
         See avahi.service(5) for more information about this configuration file -->
    
    <service-group>
        <name replace-wildcards="yes">%h</name>
    
        <service>
            <type>_daap._tcp</type>
            <port>3689</port>
            <txt-record>txtvers=1</txt-record>
            <txt-record>iTSh Version=131073</txt-record>
            <txt-record>Version=196610</txt-record>
        </service>
    
        <service>
            <type>_rsp._tcp</type>
            <port>3689</port>
        </service>
    </service-group>

Now, restart avahi and see your music server pop up automatically as a
Share in iTunes:

    sudo service avahi-daemon restart

Good Luck!

[1]: http://itunes.apple.com/app/simple-daap-client/id369605270
[2]: http://www.fireflymediaserver.org/
[3]: apt://mt-daapd

