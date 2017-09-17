---
layout: post
title: "Packaging Software with Docker"
date: 2017-09-17 10:45:37 +0200
comments: true
categories:
---

This post details how to package and deploy software with Docker.  To
try out the Alpine Linux packaged software, simply:

    docker pull troglobit/merecat

See https://hub.docker.com/r/troglobit/merecat/ for details on how to
run it in production.

<!-- more -->

The software I've chosen for this exercise is [Merecat httpd][Merecat],
and the reason is simple: we needed a *really* small web front-end to
the FTP server at work and Debian has removed [Boa][] from its repo,
which also means it's missing from Ubuntu these days.

If you've not yet installed Docker, do so now.

    sudo apt install docker.io
	sudo adduser $LOGNAME docker

**NOTE:** Remember to log out and in again to activate the new group
settings before continuing below.

Merecat comes with a [Dockerfile][], so all we need to do to is build
the image and flatten it before we deploy.  Start by cloning the repo:

    git clone https://github.com/troglobit/merecat.git
	cd merecat/
	docker build .
	[..]
	Successfully built c87dd09084a3

Take note of the resulting image hash, here `c87dd09084a3`.  You can
also find this with the `docker images` command.

Now, take it for a spin.  Allow your host's port 80 (HTTP) to be
forwarded to the container's port 80:

    docker run -dit -p 80:80 c87dd09084a3
	d98583cb31c9417c6e4117768245f1256e3003d44668e21e75589b63e0e03074
	www-browser localhost

Another hash is printed, this time for the running container, which can
also be seen using the `docker ps` command.

Finally, it's time to flatten the container:

    docker export d98583cb31c9 | docker import - troglobit/merecat:latest

Notice this size difference of between d98583cb31c9 and the flattnened
image, run `docker images`:

```
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
troglobit/merecat   latest              1764df1eb990        5 seconds ago       4.271 MB
<none>              <none>              c87dd09084a3        29 minutes ago      192.4 MB
alpine              3.5                 37c7be7a096b        3 days ago          3.991 MB
```

We now have our production image.  Kill the currently running "template"
container and remove its image before continuing:

    docker kill d98583cb31c9
	docker rmi -f c87dd09084a3

Let's check the production image, here I export my FTP directory to the
container's WWW dir.  Notice how the entry point is lost after flattening,
so we have to provide the full command, or write a new Dockerfile:

    docker run -dit -v /srv/ftp:/var/www -p 80:80 troglobit/merecat merecat -n /var/www
	www-browser localhost

You should now see the FTP server contents, provided of course your host
has an `/srv/ftp` directory.

[Boa]:        http://www.boa.org
[Merecat]:    https://github.com/troglobit/merecat
[Dockerfile]: https://github.com/troglobit/merecat/blob/master/Dockerfile

<!--
  -- Local Variables:
  -- mode: markdown
  -- End:
  -->
