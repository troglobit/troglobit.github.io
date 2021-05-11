---
title: "Emacs: GMail and Lore Mailing Lists"
subtitle: ""
orig-date: 2021-01-13 09:29:00 +0100
date: 2021-05-11 19:12:00 +0100
tags: [ notmuch, emacs, gmail, lore ]
---

With lots of help from the tireless Tobias Waldekranz, I think I've
finally found the perfect "magit like" email setup.  With Emacs, of
course.  This is the story of how I did it.

First install notmuch, mbsync (from the isync package), and afew:

```sh
    sudo apt install notmuch isync afew
```

Do initial setup of notmuch for your user.  I've opted to store my
mail in` ~/mail`, this is used throughout the text below, ymmv.

```sh
	notmuch setup
```

Clone Tobias' GitHub [notmuch-lore](https://github.com/wkz/notmuch-lore)
repo, we will be using his `pre-new` hook later on to fetch mailing
lists from lore, along with a `post-new` hook to do some tagging of new
mail.

```sh
    cd ~/src
	git clone https://github.com/wkz/notmuch-lore
```

Set up isync/mbsync to fetch my own email from Gmail.  Here a note on
Gmail folder structure may be in order.  The view you see in the Google
web interface does not reflect the IMAP hierarchy.  Here's what it looks
like, with exception for countries where Google had to change `[Gmail]`
to use `[Google Mail]` instead, e.g., Germany:

```
    /
	|-- Drafts
	|-- [Gmail]/
	|     |-- Spam
	|     |-- your-own
	|     |-- folders-here
	|     `-- ...
	|-- INBOX
	|--	Sent
	`-- Trash
```

Most of the standard standard folders are available in the root, in my
case only `Spam` goes in the same `[Gmail]/` folder as my own folders
do.  This is important to keep track of when configuring your channels
in `mbsync`.

Here's my `~/.mbsyncrc` for GMail, I've included the BusyBox folder,
which is that project's mailing list.  Filtering of incoming mail to
that list is done by GMail, not documented here.  Make sure to create
the parent directory `~/mail/gmail`.

> Please notice that I've removed my application-specific password,
> you'll have to get your own for you account.

```cfg
IMAPAccount gmail
Host imap.gmail.com
User troglobit@gmail.com
Pass heresyourappassword
SSLType IMAPS
SSLVersion TLSv1.2
CertificateFile /etc/ssl/certs/ca-certificates.crt

IMAPStore gmail-remote
Account gmail

MaildirStore gmail-local
SubFolders Verbatim
# The trailing "/" is important
Path ~/mail/gmail/
Inbox ~/mail/gmail/Inbox
Trash ~/mail/gmail/trash

Channel sync-gmail-default
Master :gmail-remote:
Slave :gmail-local:
# Select some mailboxes to sync
Patterns "INBOX"
Remove Both

Channel sync-gmail-sent
Master :gmail-remote:"Sent"
Slave :gmail-local:sent
Create Slave

Channel sync-gmail-trash
Master :gmail-remote:"Trash"
Slave :gmail-local:trash
Create Slave

Channel sync-gmail-busybox
Master :gmail-remote:"[Gmail]/BusyBox"
Slave :gmail-local:busybox
Create Slave

Channel sync-gmail-buildroot
Master :gmail-remote:"[Gmail]/Buildroot"
Slave :gmail-local:buildroot
Create Slave
Remove Both

# Get all the channels together into a group.
Group gmail
Channel sync-gmail-default
Channel sync-gmail-busybox
Channel sync-gmail-sent
Channel sync-gmail-trash
```

> **NOTE:** empty lines in `.mbsyncrc` *not allowed* just anywhere.
>           This bit me initially and is likely a common error for
>           others as well.

Test your mbsync setup first, see the manual page for details.  

As you've probably already learned from the GitHub repo, to fetch mail
from the public inbox mailing lists on lore, you need small .ini file
detailing which subsystems you're interested in:

```cfg
# ~/mail/.notmuch/.lore/sources
[netdev]
url=https://lore.kernel.org/netdev

[mtd]
url=https://lore.kernel.org/linux-mtd
```

Now we can proceed to combine it all in notmuch!  After `notmuch setup`
you should have a `~/mail/.notmuch/hooks/` directory.  Let's create a
`pre-hook` to fetch gmail and mailing lists from lore:

```sh
#!/bin/sh
# ~/mail/.notmuch/hooks/pre-new

# Fetch mail from Gmail
mbsync -a

# Pull relevant kernel mailing lists using git, of course
~/src/notmuch-lore/pre-new
```

> Remember to `chown +x pre-new` and the `post-new`, below!

For help with tagging, or sorting, you can either use the `nmsort()`
shell script function that Tobias made, or other tools like `afew`.
The latter needs a small configuration file:

```cfg
# ~/.config/afew/config
# This is the default filter chain
[SpamFilter]
[KillThreadsFilter]
[ListMailsFilter]
[ArchiveSentMailsFilter]
[InboxFilter]
```

Set up the `post-new` for notmuch as follows.  It goes the same
directory as `pre-new`:

```sh
#!/bin/sh
# ~/mail/.notmuch/hooks/post-new

nmsort()
{
    notmuch tag -new $1 -- tag:new "$2"
}

echo "Me" >&2
nmsort "+me +unread" "to:troglobit OR cc:troglobit"
nmsort "+me +unread" "troglobit"

echo "Various Mailing Lists" >&2
nmsort "+lists/pidp-11" "subject:[PiDP-11]"
#nmsort "+busybox"    "list:busybox"
#nmsort "+buildroot"  "list:buildroot"

echo "Subsystems" >&2
nmsort     "+bridge +subsys" "subject:bridge:"
nmsort  "+switchdev +subsys" "subject:switchdev:"
nmsort    "+ethtool +subsys" "subject:ethtool:"
nmsort    "+phylink +subsys" "subject:phylink:"
nmsort    "+devlink +subsys" "subject:devlink:"
nmsort        "+dsa +subsys" "subject:dsa"

echo "Hardware" >&2
nmsort    "+dsa +hw" "subject:mv88e6"
nmsort   "+mdio +hw" "subject:mdio"
nmsort    "+phy +hw" "subject:phy"
nmsort  "+mlxsw +hw" "subject:mlxsw"

echo "Vendors" >&2
nmsort   "+marvell +vendor" "subject:marvell"
nmsort "+microchip +vendor" "subject:microchip"

# If you don't use afew, uncomment this
#nmsort "" "*"

echo "Let afew handle the remaining ... " >&2
afew --tag --new
```

Set up SMTP in Emacs, also stolen from Tobias.

```lisp
;; Who am I?
(setq user-full-name "Joachim Wiberg")
(setq user-mail-address "troglobit@gmail.com")

;; Email setup
(setq message-directory "~/.mail"
      notmuch-fcc-dirs nil
      read-mail-command (quote notmuch)
      message-citation-line-format "On %a, %b %d, %Y at %R, %f wrote:"
      message-citation-line-function 'message-insert-formatted-citation-line
      )

;; Currently gmail as SMTP server
(setq message-send-mail-function 'smtpmail-send-it
      smtpmail-starttls-credentials '(("smtp.gmail.com" 587 nil nil))
      smtpmail-auth-credentials '(("smtp.gmail.com" 587 "troglobit@gmail.com" nil))
      smtpmail-default-smtp-server "smtp.gmail.com"
      smtpmail-smtp-server "smtp.gmail.com"
      smtpmail-smtp-service 587
      starttls-use-gnutls t
      )
```

Done.

