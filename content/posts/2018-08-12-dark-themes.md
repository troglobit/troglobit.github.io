---
title: "Dark Themes Ahead"
date: 2018-08-12T23:51:52+02:00
subtitle: "If we do nothing"
tags: []
---

_Reminder to self:_ How to fix Firefox `about:config` when using a Dark
Theme in Gnome or [Awesome][].

I use [System76][] amazing Gtk+ theme from [Pop!_OS][].  It's quite
simple to set up on top of Ubuntu and use in Awesome:

```shell
sudo add-apt-repository ppa:system76/pop
sudo apt update
sudo apt install pop-theme
```

I then use [gnome-tweaks][] to select the Pop-dark-slim theme.  Now, the
problem is that certain text box inputs in Firefox is either completely
dark or white and the text cannot be seen until you mark it.  This isn't
specific to the Pop!_OS theme, Adwaita-dark is exactly as bad.

<!--more-->

Here's how you fix it:

1. Open about:config in Firefox
2. Create a new String value (right click), named `widget.content.gtk-theme-override`
3. Set a light GTK theme name as the value, i chose Pop
4. Restart Firefox

Thanks to [Moritz Kammerer][] for blogging about this so I could find a solution.

[Awesome]: https://awesomewm.org/
[Pop!_OS]: https://github.com/pop-os/gtk-theme
[System76]: https://system76.com/
[gnome-tweaks]: https://wiki.gnome.org/Apps/Tweaks
[Moritz Kammerer]: https://www.mkammerer.de/blog/gtk-dark-theme-and-firefox/
