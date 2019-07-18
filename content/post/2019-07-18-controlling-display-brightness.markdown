---
title: "Controlling Display Brightness in Awesome WM"
subtitle: "Part 2/2"
date: 2019-07-18 15:13:53 +0200
tags: []
---

About a year ago I wrote about a way to control the [brightness on my
x250]({{< ref "2018-08-03-brightness-on-x250.markdown" >}}).  I found a
lot of tools, and at the time I saw a lot of potential in [light][],
which I even contributed to a lot.

However, I ended up not using the evolved versions myself, and it took
me a while to digest why.  I think the project just outgrew me and what
I wanted in such a tool.

[brightnessctl][] was another tool I had looked into, but I wanted
something much simpler, so today I wrote my own :smile:

<https://github.com/troglobit/backlight>

To use it, download the [latest release][]; then unpack, configure,
build, and install.  You may have to reboot for the udev rules to take
effect.

Users of the [Awesome][awesome] window manager can update their
`~/.config/awesome/rc.lua` to with the following:

```lua
    -- Brightness
    awful.key({ }, "XF86MonBrightnessUp", function () os.execute("backlight up") end,
              {description = "Increase brightness", group = "hotkeys"}),
    awful.key({ }, "XF86MonBrightnessDown", function () os.execute("backlight down") end,
              {description = "Decrease brightness", group = "hotkeys"}),
```

Reload Awesome and your brightness keys will now work as expected.


[light]:          https://github.com/haikarainen/light/
[brightnessctl]:  https://github.com/Hummer12007/brightnessctl/
[latest release]: https://github.com/troglobit/backlight/releases/latest
[awesome]:        https://awesomewm.org/
