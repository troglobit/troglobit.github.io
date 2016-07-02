---
layout: post
title: "Using netcat to test your Internet daemon"
date: 2016-04-21 14:21:49 +0200
comments: true
categories:
- UNIX
---

So you're having a problem with the Internet daemon you wrote.  You're
convinced the firewall, or some other magic, in your modern Linux
distribution is eating your packets.

No.

First, make sure your daemon is actually running and has successfully
bound to the address and port in question:

    sudo netstat -atnup

If your application is not listed there you have a problem with it
binding its server socket.  Check the return values from `bind()`.

If you're still suspecting the firewall, or some other magic, test your
theory with netcat.  First start a server, with your relevant address
and port, remember you need to be root, or have `CAP_NET_ADMIN`, to use
ports <= 4096:

    nc -l -u -p 9999

Here we bind to 0.0.0.0:9999. Now, start a client to test if the
server can receive any data:

    nc -u 127.0.0.1 9999

Type something on the console and press enter to send it to the server.
If you receive the data then there's no magic, execpt from bugs in you
application, preventing your appplication from working.


