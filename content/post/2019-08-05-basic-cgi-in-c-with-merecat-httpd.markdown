---
title: "Basic CGI in C with Merecat httpd"
subtitle: ""
date: 2019-08-05 22:00:25 +0200
tags: []
---


This post details how to use basic CGI programs in Merecat httpd v2.32.

Downloads:

- https://troglobit.com/projects/merecat
- http://www.boutell.com/cgic/cgic207.tar.gz

Build Merecat httpd according to the instructions in the README.  This
document assumes the directory `~/merecat/`.  Then create a config file,
`~/test.conf`, with the following content:

```
port = 8080

cgi "**.cgi|cgi-bin/*" {
    enabled = true
}
```

Since the we've chosen port 8080 we can start the server as a regular
user.  For production systems you likely want to use the default (80),
or set up proper HTTPS, see [this HowTo][howto] for help with that.

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

An stand-alone example prgram is built which we can install into the
Merecat web root.  Notice the name change to `index.cgi`, we dont't have
to use the `cgi-bin/` due to the CGI pattern we set up previously:

```
cp cgictest.cgi ~/merecat/www/index.cgi
```

Now we can start the web server and test the CGI program out.

```
cd ~/merecat/
./src/merecat -f ~/test.conf www
```

We can now open our browser at http://127.0.0.1:9000/ and there is the
CGI! :-)
