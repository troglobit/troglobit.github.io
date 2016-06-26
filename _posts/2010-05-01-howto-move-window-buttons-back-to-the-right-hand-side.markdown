---
layout: post
title: "HowTo: Move window buttons back to the right"
date: 2010-05-01 16:46:57 +0200
comments: true
categories: 
- Ubuntu
- HowTo
---

When you've installed Ubuntu 10.04 you might want to have the window
controls back to the right side, where you're used to having them.  Use
the following simple command:

    gconftool-2 --set /apps/metacity/general/button_layout --type string "menu:minimize,maximize,close"

