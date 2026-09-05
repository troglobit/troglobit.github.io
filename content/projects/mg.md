---
name: mg
title: "Micro Emacs for the terminal"
date: 2026-09-05 10:00:00 +02:00
lastmod: 2026-09-05 10:00:00 +02:00
aliases: [ /mg.html, /mg ]
image: /images/mg-card.png
description: "mg is a small, fast, and portable Emacs-like editor with syntax highlighting and UTF-8 support, descended from MicroEMACS by way of OpenBSD, and in the public domain."
tags: [ mg, emacs, editor, opensource ]
---

`mg` is a small, fast, and portable Emacs-like editor for people who
cannot, or would rather not, run the real thing.  It keeps the GNU Emacs
key bindings, so there is no second flavour to learn, and it builds with
nothing but a C compiler.  Like its ancestors it is in the public domain.

<!--more-->

This project is derived from, and tracks, [OpenBSD Mg][upstream], adding
usability features while staying friendly to resource constrained
systems.

Features
--------

- Emacs key bindings, with a quick help card on `C-h q` and a tutorial
  on `C-h t`, both built into the binary
- Syntax highlighting for C and C++, shell, make, python, markdown,
  YAML, configuration files, diffs, and git commit messages.  The mode
  follows the file, and `M-x font-lock-mode` turns the colours off
- UTF-8 in UTF-8 locales: multibyte text is typed, displayed, and
  edited as characters, and double-width CJK takes the two columns it
  is drawn in
- Visual mark mode, where the region between mark and dot is shown in
  reverse video, and shift-select with the arrow keys
- Side by side windows with `C-x 3`, moving between them with the Meta
  arrow keys, resizing with Meta-Shift, and `C-x +` to balance
- Line wrap, so a long line continues on the rows below instead of
  running off the edge
- Smart Tab that indents the line, or the region when the mark is
  active, in the language modes that have an opinion about indentation
- Builds without curses, on termios and escape sequences alone, for
  systems where terminfo is too much
- Reads `tags` files, opens gzipped text read-only, and keeps a
  `(row,col)` modeline like Emacs

Language Modes
--------------

A mode is picked from the file name, or from the `#!` line when there
is one, and decides both the colours and how the buffer behaves.  A
makefile gets hard tabs because recipes need them, python and YAML get
spaces because a tab is a syntax error there, and a commit message gets
the keys for finishing, cancelling, and signing off.

Add your own in `~/.mg`:

    auto-execute *.rst text-mode
    auto-execute *.lua shell-script-mode

Keys
----

Emacs notation throughout: `C-` is Control, `M-` is Meta, which today
is `Alt` or a tap on `Esc`.  So `C-x C-c` means hold Control and tap
`x` then `c`, and `C-h q` means hold Control and tap `h`, release, then
tap `q`.

If you have never used Emacs, `C-h t` opens the tutorial and walks you
through it.

Get It
------

> [Latest release][releases] available on [GitHub][repo]

* [Repository][repo]
* [Download release tarballs][releases]
* [Release notes / ChangeLog][changelog]
* [Issue Tracker][issues]
* Man page: [mg.1][man-mg]

Users who clone from GitHub must run `./autogen.sh` first; released
tarballs already ship a `configure` script, so a C compiler and `make`
are all you need.

Origin & References
-------------------

Mg is a [Micro Emacs][] clone created in 1987, based on the [MicroEMACS][]
v30 that Dave Conroy released in 1985.  The name, "Micro GNU Emacs", was
disputed early on by the FSF, so today it simply goes by `mg`.

The [OpenBSD][upstream] tree has carried the best maintained version of
it for years, and this project follows that tree, porting the fixes back
and developing the parts a terminal editor wants in 2026.  Unlike most
[ErsatzEmacs][] clones it aims at compatibility with GNU Emacs rather
than at a dialect of its own.

The code is free of any licence at all: it is in the [public
domain][license].

[Micro Emacs]: https://www.emacswiki.org/emacs/MicroEmacs
[MicroEMACS]:  https://github.com/troglobit/MicroEMACS
[ErsatzEmacs]: https://www.emacswiki.org/emacs/ErsatzEmacs
[upstream]:    https://cvsweb.openbsd.org/src/usr.bin/mg/
[repo]:        https://github.com/troglobit/mg
[releases]:    https://github.com/troglobit/mg/releases
[changelog]:   https://github.com/troglobit/mg/blob/master/ChangeLog.md
[issues]:      https://github.com/troglobit/mg/issues
[license]:     https://unlicense.org/
[man-mg]:      https://man.troglobit.com/man1/mg.1.html
