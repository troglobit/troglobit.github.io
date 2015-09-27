---
layout: post
title: "Rant: All the C Compiler Warnings ..."
date: 2015-01-12 01:57:40 +0100
comments: true
categories: programming
---

> ``Enable *all* the warnings!''

This is one of the truths you learn when you start working with C.  Most
of the time adding `CPPFLAGS="-W -Wall -Werror"` is all you need to find
all the nasty bugs.  And if that's not enough, there are tons of tools
for static code analysis, like `scan-build` in
[Clang](http://clang-analyzer.llvm.org/scan-build.html), and
[Coverity Scan](https://scan.coverity.com/), to help you *find all the
bugs*!

However, these pesky warnings (some of which cannot even be disabled!)
are sometimes more of a nuisance than help.  Sometimes you *know* that
some parameters to a function will remain unused -- it's a callback, and
you don't need all the data given to you.  So you start adding all kinds
of voodoo, like `__attribute__ ((unused))` ... seriously?

<!-- more -->

Imagine this now sprinkled all over the code base.

```C
    static void callback(uev_ctx_t *ctx __attribute__((unused)),
                         uev_t *w __attribute__((unused)),
                         void *arg __attribute__((unused)),
                         int events __attribute__((unused)))
    {
            ...
    }
```

So you make small macros to help out:

```C
    #define UNUSED(arg) arg __attribute__ ((unused))
```

We now have this instead:

```C
    static void callback(uev_ctx_t *UNUSED(ctx), uev_t *UNUSED(w),
                         void *UNUSED(arg), int UNUSED(events))
    {
            ...
    }
```

Admittedly, using the macro is much cleaner, but still.  Why not just
allow this *much more readable* version?

```C
    static void callback(uev_ctx_t *ctx, uev_t *w, void *arg, int events)
    {
            ...
    }
```

The human eye is marvellous at finding little things that stick out,
but when there's nothing but annoying things on the screen we can no
longer see the most obvious pointer casting bugs, and we get sloppy.

I'm seriously considering adding some kind of `--developer-mode` to my
own projects which will warn like crazy, as usual, and as soon as I turn
it off (default) complete silence will arrive.  That's how sick and
tired I am of *all the warnings* and the resulting almost completely
unreadable code.

[We spend a lot of time reading code](http://blog.codinghorror.com/when-understanding-means-rewriting/),
so it should be enjoyable and easy.


