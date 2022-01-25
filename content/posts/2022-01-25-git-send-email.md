---
title: "Gmail and git send-email"
date: 2022-01-24 19:37:44 +0200
tags: [ howto, git, opensource ]
draft: false
---

As a follow-up to my previous Buildroot mailing-list post about the
`git format-patch` and `git send-email` commands, this post covers how
to set up the latter.

<!--more-->

In many corporate settings it's either hard or close to impossible to
rely on the services of the IT dept. to send email to Open Source
mailing lists.  In this blog post we will set up Google Mail (Gmail)
as an alternative.

  - Gmail, because many people already have an account
  - Easy to add "another" account: `yourusername+oss@gmail.com`
  - Doesn't add any obnoxious corporate "CONFIDENTIAL" footer
  - Your contributions are yours:
    - Remember attribution to your Company, this is usually a good
      idea for several reasons, set the `Organization:` email header
  - However, **avoid the web client:**
    - Changes tabs to spaces
    - Wraps text (patch text) unconditionally
    - Base64 encodes messages with non US ASCII characters: åäö
  - Instead, **use with external client**:
    - Get [application specific password](https://myaccount.google.com/apppasswords)
    - Enable [IMAP for fetching email](https://mail.google.com/mail/u/0/#settings/fwdandpop)

> Tips for how to set up your external email client are available here
> <https://www.kernel.org/doc/html/latest/process/email-clients.html>,
> or check out another blog post about using Emacs with Notmuch Mail:
> https://troglobit.com/post/2021-05-11-emacs-gmail-and-lore-mailing-lists/

Then edit your `~/.gitconfig` using your favorite editor:

```
[user]
    name           = YOUR NAME
    email          = yourusername+oss@gmail.com
[format]
    headers        = "Organization: Company Inc\n"
    signoff        = true
[sendemail]
    chainreplyto   = false
    smtpserver     = smtp.gmail.com
    smtpuser       = yourusername@gmail.com
    smtpencryption = tls
    smtpserverport = 587
    smtpPass       = yourapplicationspecificpassword
    aliasesfile    = ~/.config/mutt/aliases
    aliasfiletype  = mutt
[sendemail.netdev]
    to             = "davem@davemloft.net, kuba@kernel.org"
    cc             = "netdev@vger.kernel.org"
    cccmd          = "./scripts/get_maintainer.pl --nogit --nogit-fallback --norolestats"

# You can also use "alias-foo" in to= and cc=, expands from ~/.config/mutt/aliases
```

This fils applies to all your GIT repositories.  If you want to have a
different behavior or setting for a repoistory, see the `.git/config`
file for that specific working copy.  These file holds other settings,
see the man page [git-config(1)](https://git-scm.com/docs/git-config)
for details.

One specific detail deserves to be highlighted.  In the `.gitconfig`
above, there is a section `[sendemail.netdev]`.  It is triggered by
the `--identity` argument:

    $ git send-email --identity netdev mail/*

The extra settings (or overrides) defined for `netdev` are then used
for all `git send-email` commands with that identity.  This is useful
in particular if you interact with many mailing-lists, but also when
you are old and forgetful like yours truly.

Good Luck!

> **Note:** see my previous post, the [Buildroot Checklist][], for an
> introduction to how to use `git format-patch`.  It applies to many
> other mail-list based Open Source projects as well.

[Buildroot Checklist]: https://troglobit.com/post/2022-01-04-buildroot-development-checklist/
