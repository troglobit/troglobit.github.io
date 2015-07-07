---
layout: post
title: "Link Collection w31 2008"
date: 2008-07-30 16:22:36 +0200
comments: true
categories: 
---

George Dyson, son of legendary [Freeman Dyson][1], talks about
[the first computer][2], the first software bugs (both physical and
logical) and the initial struggles of hackers.  Fun history lesson for
computer engineers and programmers alike.  (Now, go crawl the web for
"Dyson Sphere" and "Star Trek"! :-)

Is Ubuntu 8.04 really that buggy as everyone suggest?  My guess is that
we've reached a breaking point where beginner users (< 1 year) are
starting to outnumber the older "hard core" users.  Or have the top 20%
moved on to other distros?  [Even XKCD has picked up on it&nbsp;...][3]

[Michael Meeks][4] of Novell was interviewed recently by the Austrian
paper [derStandard.at][5], and one of the things he mentioned was the
[OpenOffice fork][6] they maintain, very interesting new features, not
(yet) included in OpenOffice.org, e.g. .docx and VBA support!

A couple of days ago I managed to convince [a friend][7] of mine to try
running bleeding edge GNU Emacs from CVS.  He almost gave up, kicking
and screaming, due to his Bitstream Vera fonts becoming totally screwed
up compared to [Alexandre Vassalottis snapshot build][8] from January.
It turned out to be caused by the Emacs font back-end
[defaulting to the old X font renderer][9] rather than the new XFT one.

Here is [my ~/.Xresources][10] file that seems to fix the problem.  Run
`xrdb -merge ~/.Xresources` to merge in the new settings without logging
out/in again.

The [Canonical][11] [SourceForge][12] equivalent, [Launchpad][13], is
slowly turning into something really cool.  [Take the tour][14] if you
are curious and want to know more.

I cannot believe I haven't heard of [pastebin][15] before.  Thanks
[Rooth][16]!

My [dear wife][17] is a [GNU Nano][18] user.  Here are a
[couple of tips][19] for her, and other die-hard Nano users.

Are you an electronics or computer engineer?  Then you've probably had
trouble explaining boolean logic to people.
[This dude explains it all using dominoes.][20]

Finishing off with this, unbeatable, hardware hacker.  He's transformed
his [EeePC][21] into a veritable monster!  [See his guide][22] for the
changes necessary&nbsp;...

[1]: http://en.wikipedia.org/wiki/Freeman_Dyson
[2]: http://www.ted.com/index.php/talks/george_dyson_at_the_birth_of_the_computer.html
[3]: http://xkcd.com/456/
[4]: http://www.gnome.org/~michael/
[5]: http://derstandard.at/?url=/?id=1216917892794
[6]: http://go-oo.org/discover/
[7]: http://vmlinux.org/mwik/
[8]: http://peadrop.com/blog/category/computers/emacs/
[9]: http://lists.gnu.org/archive/html/emacs-devel/2008-06/msg01224.html
[10]: http://vmlinux.org/jocke/myblog/entries/misc/my.Xresources
[11]: http://www.canonical.com/
[12]: http://www.sf.net/
[13]: http://www.launchpad.net/
[14]: https://launchpad.net/+tour/index
[15]: http://www.pastebin.com/
[16]: http://kalkig.nu/
[17]: http://vmlinux.org/ilse/
[18]: http://www.nano-editor.org/
[19]: http://nullcortex.com/2008/07/28/nano-nano/
[20]: http://www.youtube.com/watch?v=SudixyugiX4
[21]: http://en.wikipedia.org/wiki/Asus_Eee_PC
[22]: http://beta.ivancover.com/wiki/index.php/Eee_PC_Internal_Upgrades
