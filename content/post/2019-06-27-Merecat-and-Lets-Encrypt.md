---
title: "HowTo: Set up Merecat with Let's Encrypt certificate"
date: 2019-06-27T16:27:39Z
subtitle: ""
tags: []
---

This is a HowTo for setting up [Merecat httpd][merecat] with [Let's
Encrypt](https://letsencrypt.org/) HTTPS certificates.

The upcoming v2.32 release of [Merecat][] supports HTTPS as well as
serving more than one Internet port.  This is highly useful for those
who want to serve both HTTPS and HTTP content.

> **Update:** now with support for `--webroot` and HTTP-01 renewal!

<!--more-->

## Getting the Source

To start with, you need the latest release of [Merecat][].  Note, if you
are reading this before Merecat v2.32 has been released you can use the
latest software from the GitHub master branch.  Note, you need OpenSSL
and a few other packages, see the README file for details:

```sh
git clone https://github.com/troglobit/merecat.git
cd merecat/
./autogen.sh
./configure
make package
sudo dpkg -i ../merecat_2.32-1_amd64.deb
```

## Starting Certbot

If you already have at least version v2.32 of [Merecat][] installed you
can begin the process of getting a Let's Encrypt certificate.  This
HowTo use the EFF's certbot, which is available in Ubuntu or from the
Let's Encrypt homepage:

<pre>
root@example:/var/www# <b>systemctl stop merecat.service</b>
root@example:/var/www# <b>certbot certonly --standalone</b>
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Plugins selected: Authenticator standalone, Installer None
Enter email address (used for urgent renewal and security notices)  (Enter 'c'
to cancel): <b>admin@example.com</b>

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Please read the Terms of Service at
https://letsencrypt.org/documents/LE-SA-v1.2-November-15-2017.pdf. You must
agree in order to register with the ACME server at
https://acme-v02.api.letsencrypt.org/directory
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(A)gree/(C)ancel: <b>a</b>

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Would you be willing to share your email address with the Electronic Frontier
Foundation, a founding partner of the Let's Encrypt project and the non-profit
organization that develops Certbot? We'd like to send you email about our work
encrypting the web, EFF news, campaigns, and ways to support digital freedom.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: <b>y</b>
Please enter in your domain name(s) (comma and/or space separated)  (Enter 'c'
to cancel): example.com *.example.com
Obtaining a new certificate
Performing the following challenges:
Client with the currently selected authenticator does not support any combination of challenges that will satisfy the CA. You may need to use an authenticator plugin that can do challenges over DNS.
Client with the currently selected authenticator does not support any combination of challenges that will satisfy the CA. You may need to use an authenticator plugin that can do challenges over DNS.

IMPORTANT NOTES:
 - Your account credentials have been saved in your Certbot
   configuration directory at /etc/letsencrypt. You should make a
   secure backup of this folder now. This configuration directory will
   also contain certificates and private keys obtained by Certbot so
   making regular backups of this folder is ideal.
</pre>

Oups, that didn't go as planned ... well, it turns out the version of
certbot in Ubuntu 18.04 lacks support for *wildcard certificates*.  We
can either go to their homepage and get the latest version, or list all
the domains we need:

<pre>
root@example:/var/www# <b>certbot certonly --standalone</b>
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Plugins selected: Authenticator standalone, Installer None
Please enter in your domain name(s) (comma and/or space separated)  (Enter 'c'
to cancel): <b>example.com www.example.com git.example.com ftp.example.com deb.example.com</b>
Obtaining a new certificate
Performing the following challenges:
http-01 challenge for example.com
http-01 challenge for deb.example.com
http-01 challenge for ftp.example.com
http-01 challenge for git.example.com
http-01 challenge for www.example.com
Waiting for verification...
Cleaning up challenges

IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/example.com/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/example.com/privkey.pem
   Your cert will expire on 2019-09-24. To obtain a new or tweaked
   version of this certificate in the future, simply run certbot
   again. To non-interactively renew *all* of your certificates, run
   "certbot renew"
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le
</pre>

The [Let's Encrypt setup guide][letsguide] at this point recommends that
we verify that automatic certificate renewal works (cronjob or systemd
timer).  If this doesn't work, your certificate will expire after only
six months!  So let's check that now:

<pre>
root@example:/var/www# <b>certbot renew --dry-run</b>
Saving debug log to /var/log/letsencrypt/letsencrypt.log

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Processing /etc/letsencrypt/renewal/example.com.conf
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Cert not due for renewal, but simulating renewal for dry run
Plugins selected: Authenticator standalone, Installer None
Renewing an existing certificate
Performing the following challenges:
http-01 challenge for deb.example.com
http-01 challenge for ftp.example.com
http-01 challenge for git.example.com
http-01 challenge for merecat.example.com
http-01 challenge for example.com
http-01 challenge for www.example.com
Waiting for verification...
Cleaning up challenges

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
new certificate deployed without reload, fullchain is
/etc/letsencrypt/live/example.com/fullchain.pem
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
** DRY RUN: simulating 'certbot renew' close to cert expiry
**          (The test certificates below have not been saved.)

Congratulations, all renewals succeeded. The following certs have been renewed:
  /etc/letsencrypt/live/example.com/fullchain.pem (success)
** DRY RUN: simulating 'certbot renew' close to cert expiry
**          (The test certificates above have not been saved.)
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

IMPORTANT NOTES:
 - Your account credentials have been saved in your Certbot
   configuration directory at /etc/letsencrypt. You should make a
   secure backup of this folder now. This configuration directory will
   also contain certificates and private keys obtained by Certbot so
   making regular backups of this folder is ideal.

root@example:/var/www#
</pre>

It is recommended to also set up Diffie-Hellman paramaters.  For this
you need to genereate a site specific file:

```sh
root@example:/var/www# openssl dhparam -out certs/dhparam.pem 2048
Generating DH parameters, 2048 bit long safe prime, generator 2

This is going to take a long time
........................................................+.........................................................................
.......+......................................+...................................................................................
.............................................................................................+....................................
..................................................................................................................................
......................................................................+...........................................................
............................................................................................................................+.....
..................................................................................................................................
.+.......+........................................................................................................................
.........................+........................................................................................................
.................+.............................................................++*++*++*++*
root@example:/var/www#
```

The `/etc/merecat.conf` file for a site with a couple of virtual hosts,
look like this:

```conf
## /etc/merecat.conf                                     -*-conf-unix-*-

virtual-host     = true

user-agent-deny  = "**SemrushBot**|**MJ12bot**|**DotBot**"

server default {
    port     = 80
}

server secure {
    port     = 443
    ssl {
        certfile = /etc/letsencrypt/live/example.com/fullchain.pem
        keyfile  = /etc/letsencrypt/live/example.com/privkey.pem
        dhfile   = certs/dhparam.pem
    }
}
```

The [Merecat][] defaults take care of a lot of the nasty details you
shouldn't have to bother with, and unlike other web servers the virtual
host setup is done in the file system rather than in the configuration
file.  See [Merecat][] docs for details.

With everything set up we can fire it up:

<pre>
root@example:/var/www# <b>systemctl start merecat.service</b>
</pre>

## HTTP-01 Renewal

For automatic renewal to work you need a new (July 2020) feature of
Merecat, called location matching:

```conf
server default {
        port = 80
        location "/.well-known/acme-challenge/**" {
                 path = "letsencrypt/.well-known/acme-challenge/"
        }
        redirect "/**" {
                 code = 301
                 location = "https://$host$request_uri$args"
        }
}
```

The `path` must be relative to the server root directory.  Use bind
mounts to get `/var/lib/letsencrypt` into your server root.  This way
we can ensure `certbot` only writes to its own directory and cannot
write to any file in the server root.

Then run `certbot` with the following arguments and then add all virtual
hosts you want to support from Merecat:

```shell
root@example:/var/www/> certbot certonly --webroot --webroot-path /var/lib/letsencrypt
```


[Merecat]: https://merecat.troglobit.com
[letsguide]: https://certbot.eff.org/lets-encrypt/ubuntubionic-other
