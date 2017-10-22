---
categories:
- Merecat
- thttpd
- httpd
- HTTPS
- OpenSSL
comments: true
date: 2017-02-12T00:07:44Z
title: HTTPS proxy for Merecat httpd
url: /2017/02/12/https-proxy-for-merecat-httpd/
---

This is a HTTPS proxy HowTo for [Merecat httpd][merecat] using [pound][]
and OpenSSL.

Pound is a reverse proxy, load balancer, and HTTPS front-end for Web
servers.  It is available in Debian/Ubuntu and is very simple to set up:

First install the package, including OpenSSL, or LibreSSL:

    sudo apt install pound openssl

Use OpenSSL to create a self-signed certificate:

    mkdir ~/certs
    cd  ~/certs
	openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes
    cat cert.pem key.pem > bundle.pem

Now, we move to the Merecat directory from the previous blog post and
start it on port 8080:

    cd ~/merecat
    ./src/merecat -p 8080 www/

Now, edit the default `/etc/pound/pound.cfg` to include the following:

    ListenHTTPS
        Address 0.0.0.0
        Port 443
        AddHeader "X-Forwarded-Proto: https"
        AddHeader "X-Forwarded-Port: 80"
        HeadRemove "X-Forwarded-Proto"
        HeadRemove "X-Forwarded-For"
        Cert "/home/jocke/certs/bundle.pem"

        # This is the address and TCP port where Merecat httpd runs
        Service
                BackEnd
                        Address 127.0.0.1
                        Port 8080
                End
        End
    End

We make sure to remove any existing `X-Forwarded-For` header to prevent
any malicious client from injecting them beforehand.  Then enable pound
by editing `/etc/default/pound`

    startup=1

And start the service

    sudo /etc/init.d/pound restart

Your service is now available over HTTPS.  Try it with curl, which needs
to be called with `-k` to skip certificate validation:

    curl -ki https://localhost/~jocke/
    HTTP/1.0 200 OK
    testing stderr
    Content-Type: text/html;charset=utf-8
    
    <html>
    <head><title>Hej</title></head>
    <body>
    <p>Hello, HTTP SPOKEN HERE</p>
    </body></html>
    
All done. Good Luck!

[pound]:   http://www.apsis.ch/pound/
[merecat]: http://merecat.troglobit.com


<!--
  -- Local Variables:
  -- mode: markdown
  -- End:
  -->
