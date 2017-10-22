---
categories:
- Ubuntu
- HowTo
comments: true
date: 2010-05-01T16:46:57Z
title: 'HowTo: Move window buttons back to the right'
url: /2010/05/01/howto-move-window-buttons-back-to-the-right-hand-side/
---

When you've installed Ubuntu 10.04 you might want to have the window
controls back to the right side, where you're used to having them.  Use
the following simple command:

    gconftool-2 --set /apps/metacity/general/button_layout --type string "menu:minimize,maximize,close"

