---
title: "Your Own Operating System"
date: 2022-04-17 16:56:00 +0100
tags: [ howto, buildroot, git, opensource ]
draft: false
---

This post shows how you can create your own operating system based on
[Buildroot][].  Add your own packages, patches, and your very own flavor
to the experience.

> My own take on this is [myLinux][], which started out as a TroglOS,
> and before that as [miniroot][], by Henrik Nordström.  Please feel
> free to dig around myLinux for more inspiration and tips on how to
> solve common use-cases.

<!-- more -->

The [manual][], section 9, describes the basic process of setting up a
our customizations as a `BR2_EXTERNAL` tree.  Let's start with the
basic directory layout.  We've also chosen a name for our little system:
"Foo".

    mkdir foo
    cd foo
    git init
    touch Makefile
    touch Config.in
    touch external.mk
    touch external.desc
    mkdir configs
    touch configs/foo_defconfig
    git add .

Edit `external.desc`, notice the name in CAPITAL letters:

    name: FOO
    desc: Foo System

Now, we want our project to be stand-alone, so we add buildroot as a
GIT submodule to our project:

    git submodule add https://github.com/buildroot/buildroot.git

To make things easier for us, we're also creating a Makefile to set some
of the critical buildroot environment variables needed.  For details,
see the [manual][].  Edit `Makefile`:

```Makefile
export BR2_EXTERNAL := $(CURDIR)
export PATH         := $(CURDIR)/bin:$(PATH)

ARCH ?= $(shell uname -m)
O    ?= $(CURDIR)/output

config := $(O)/.config
bmake   = $(MAKE) -C buildroot O=$(O) $1


all: $(config) buildroot/Makefile
	@+$(call bmake,$@)

$(config):
	@+$(call bmake,list-defconfigs)
	@echo "ERROR: No configuration selected."
	@echo "Please choose a configuration from the list above by running"
	@echo "'make <board>_defconfig' before building an image."
	@exit 1

%: buildroot/Makefile
	@+$(call bmake,$@)

buildroot/Makefile:
	@git submodule update --init
```

[Buildroot]: https://buildroot.org
[manual]:    https://buildroot.org/downloads/manual/manual.html
[myLinux]:   https://github.com/troglobit/myLinux
[miniroot]:  https://github.com/hno/miniroot
