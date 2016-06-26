---
layout: post
title: "Suddenly Compiz is not Working Anymore ..."
date: 2008-07-17 01:48:05 +0200
comments: true
categories: 
---

So weird.  I usually rearrange my desktop every two weeks, often when I
am bored.  Sometimes I want a quick lean, smallish desktop and other
times I want the whole shebang, all possible animations, SVG icons,
mouse gestures — you name it and I will already have tons of it!

Today I wanted to enable Compiz again and it just wouldn't start.  After
a couple of tries that turned out to be dead ends I finally got this:

    /usr/bin/compiz.real (core) - Error: Could not acquire compositing manager selection on screen 0 display ":0.0" 

Some more digging around Google gave me the answer: Metacity and Compiz
fight to be "compositing manager" ...

... as soon as I disabled `/apps/metacity/general/compositing_manager` I
could enable Compiz again!

