---
categories:
- Emacs
comments: true
date: 2008-02-24T10:59:19Z
title: Enterprise Emacs
url: /2008/02/24/enterprise-emacs/
aliases: /blog/2008/02/24/enterprise-emacs/
---

Many years ago I discovered the beauty in a beast called [Emacs][1].  I
am actually a frequent user of both Emacs and [Vim][2], but I firmly
belive in the notion of [learning one editor well][3]:

> "The editor should be an extension of your hand; make sure your editor
> is configurable, extensible, and programmable." &mdash;&nbsp;[The Pragmatic Programmer][3]

At the many jobs I have had, colleagues often glanced over my shoulder
and said; "Oh, Emacs&nbsp;... Yeah I used that ages ago when I was
working on UNIX&nbsp;...", often they remember it fondly, sometimes for
all the quirky keyboard shortcuts.  Very few know that it is still being
actively developed.

Emacs can be quite counter intuitive and sometimes even an outright pain
to use.  I find it a shame that still today, after (literally) decades
there are no sane defaults.  Once, when I was still forced to use
Windows, I saw a setup wizard in [Win32-Emacs][4] that resembled what I
would like to have — a sort of use cases possible to chose from.  It may
have been some extension that was maintained in some non-official
version, because now when I look for it I cannot find it.  But why not
have a setup wizard in the upstream distribution of Emacs as well?

Well, this is my gripe, the pieces of Emacs that are still unfriendly to
users, and mostly new users, coming from Windows or MAC.  I will use
this blog to present ideas and small things I have done to make Emacs
more user friendly.  I call this *Enterprise Emacs*.

<!--more-->

Many of the following, really obvious, settings are today available in a
menu called "Options", some are still buried deep within the scriptable
innards and go by unfamiliar names.

    ;; Enterprise Emacs -- save this as your ~/.emacs
    ; Scrollbar on the *right*, or none (use 'nil for none)
    (set-scroll-bar-mode 'right)
    
    ; A visible marked region
    (transient-mark-mode t)
    
    ; Marking text with shift + arrow keys
    (pc-selection-mode t)
    (delete-selection-mode t)
    
    ; Syntax highlighting
    (global-font-lock-mode t)
    
    ; A cursor that blinks so I can find it
    (blink-cursor-mode t)
    
    ; A status line displaying both line *and column* of my cursor
    (column-number-mode t)
    
    ; Continue at the file and position where I left off when I closed Emacs
    (desktop-save-mode t)

In the coming months I will present some useful tips from my bag of
tricks.  Including, but not limited to:

* Auto-adjust as you type
* Auto-spell as you type, text and code
* Auto-complete words and function/method names
* A solution to: "Where the h-ll is that function?"
* Debugging from inside Emacs, like an IDE!
* Windows-like marking, with Ctrl-x, Ctrl-v and Ctrl-v 

Plus the usual Emacs features that users of Microsoft products may not
have ever known.  To mention a few: complete indentation engine, with
several predefined indentation modes.  Built-in calculator (converts
between bases!)

The first tip is Emacs-23 with (grab on to something!)  anti-aliasing of
fonts!  Yes, it's still an experimental feature, and if you did not
already know this, "experimental" in free/open source is often quite
stable.  I have used it daily for >6 months and it has crashed on me
only once.

* [Ubuntu][5]
* [Debian][6]

Start off by installing it and discover the Options menu.  Then return to
this blog and I will have the next installment ready.

[1]: http://www.gnu.org/software/emacs/
[2]: http://www.vim.org
[3]: http://pragmaticprogrammer.com/the-pragmatic-programmer/extracts/tips
[4]: http://ntemacs.sourceforge.net/
[5]: http://peadrop.com/blog/2007/01/06/pretty-emacs/
[6]: http://emacs.orebokech.com/
