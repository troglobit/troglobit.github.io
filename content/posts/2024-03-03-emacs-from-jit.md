---
title: Building Emacs with JIT
date: 2024-03-03T13:19:28Z
tags: [ emacs, gcc, jit ]
---

Very brief intro to building GNU Emacs from GIT, with GCC JIT enabled to
greatly speed up our favorite editor, [GNU Emacs][]!

### Perquisites

You need a lot of development packages installed to check out and build Emacs.
How to install these are outside the scope of this blog post.  The output of
the `configure` script and some intuition is usually sufficient.  On Debian,
Ubuntu, or Linux Mint, systems at least the following is needed:

     sudo apt install build-essential autoconf automake git make      \
              libgtk-4-dev libwebkitgtk-6.0-dev libxpm-dev libgif-dev \
              libjpeg-dev libxft-dev libotf-dev librsvg2-dev libwebp-dev

The one package *critical* though for the JIT feature is:

    sudo apt install libgccjit-11-dev

> **Note:** the version of the package depends on what version of GCC is your
> primary, usually selected by your Linux distribution.

### Checkout & Build

To build Emacs we first have to clone it using GIT from the GNU repository:

    cd src/
    git clone https://git.savannah.gnu.org/git/emacs.git

Now we can start, first we create the `configure` script and `Makefile.in`'s,
because they are generated files and not checked into GIT:

    cd emacs/
    ./autogen.sh

Finally, time to configure the features we want.  I've selected a few that you
may not want, see `./configure --help` for more information:

    ./configure --with-native-compilation --with-xwidgets --with-tree-sitter --with-xft

The features you select with the `configure` script also mandate that you have
development versions of the libraries (packages) providing these features.
See above `apt install` for hints on how to install packages.  I've selected:

 - [Native compilation](https://www.emacswiki.org/emacs/GccEmacs) (JIT)
 - [X Widgets](https://www.emacswiki.org/emacs/EmacsXWidgets), with WebKit GTK
 - [Tree Sitter](https://www.emacswiki.org/emacs/Tree-sitter)
 - [Xft](https://www.emacswiki.org/emacs/XftGnuEmacs)

Now, Emacs consists of both C code and LISP code (the Emacs Lisp dialect), so
not unlike when building a compiler, we need to bootstrap Emacs first so that
it can build itself:

    make bootstrap

After this initial step, which takes several minutes, we can go ahead and build
the final stages:

    make

### Installation

I recommend you remove any Emacs packages, including elpa, or similar `*-el`
packages, that you may have installed already.  Otherwise you risk getting
incompatible Elvisp^Welisp package in your system.

When you've cleaned up your system, it's time to install Emacs:

    sudo make install

The default install location is `/usr/local`, with the Emacs binary installed
in `/usr/local/bin`.  If you want another location, you can control that with
the `--prefix` option to configure (above).

### Setup

The basic setup of GNU Emacs, out of the box, is better than it was 20 years
ago, but you'll likely want to spend some time setting it up to your liking:

 - Modern copy-paste (CUA mode)
 - Saving state of files/cursor/etc. between sessions
 - Ido Mode <https://www.masteringemacs.org/article/introduction-to-ido-mode>
 - ...

If you want to have a look at my dark-mode-c-python-programming `~/.emacs` as
an example: <https://github.com/troglobit/toolbox/blob/master/dot.emacs>

[GNU Emacs]: https://www.gnu.org/software/emacs/
