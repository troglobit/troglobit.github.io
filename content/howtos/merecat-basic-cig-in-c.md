---
title: "Basic CGI in C with Merecat httpd"
subtitle: ""
date: 2019-08-05 22:00:25 +0200
aliases: [/posts/2019-08-05-basic-cgi-in-c-with-merecat-httpd/]
tags: [merecat]
---


This post is a writeup of how to use basic CGI programs in Merecat httpd v2.32.

<!--more-->

Downloads:

- https://troglobit.com/projects/merecat
- http://www.boutell.com/cgic/cgic207.tar.gz

Build Merecat httpd according to the instructions in the README.  This
document assumes the directory `~/merecat/`.  Then create a config file,
`~/test.conf`, with the following content:

```conf
port = 8080

cgi "**.cgi|cgi-bin/*" {
    enabled = true
}
```

In this example we've chosen port 8080 so we can start the server as a
regular user.  For production systems you likely want to use the default
(80), or set up proper HTTPS, see [Let's Encrypt Merecat][howto] for
help with that.

The classic _cgic_ used here is just an example.  A CGI program can be
written in just about any programming language, or plain shell script.
Merecat actually ships with a `/cgi-bin/printenv`, which is a plain
Bourne shell script.

Download, unpack and build cgic:

```
mkdir temp; cd temp/
wget http://www.boutell.com/cgic/cgic207.tar.gz
tar xf cgic207.tar.gz
cd cgic207/
make
```

A stand-alone example program is built which we can install into the
Merecat web root.  Notice the name change to `index.cgi`, we don't have
to use the `cgi-bin/` due to the CGI pattern we set up previously:

```
cp cgictest.cgi ~/merecat/www/index.cgi
```

Now we can start the web server and test the CGI program out.

```
cd ~/merecat/
./src/merecat -f ~/test.conf www
```

We can now open our browser at http://127.0.0.1:8080/ and there is the
CGI!  Remember to check out http://127.0.0.1:8080/cgi-bin/printenv too.

[howto]: {{< relref "/howtos/merecat-and-lets-encrypt.md" >}}
