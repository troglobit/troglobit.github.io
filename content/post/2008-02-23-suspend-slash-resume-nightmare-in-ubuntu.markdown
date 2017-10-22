---
categories:
- ubuntu
comments: true
date: 2008-02-23T21:34:44Z
title: Suspend/Resume Nightmare in Ubuntu
url: /2008/02/23/suspend-slash-resume-nightmare-in-ubuntu/
---

I've had my ThinkPad T43 for a while now and I'm really pleased with it,
everything just works!  It took some pleading and, I admit, begging to
persuade my boss and the IT department to buy it since they usually only
buy from Dell or HP.  This was mainly due to care packs and payment plans
that these suppliers offer companies.  My ThinkPad they had to actually
pay for straight up.  So if I were a Lenovo sales person, that was
something I would fix to boost my sales.

This post is however not about sales, brands of laptops or how the cat
ate my homework ... well, OK, almost.  Ubuntu/Gnome ate my resume!

When I initially installed the laptop, early 2006, I went with
Ubuntu 5.10.  Ubuntu is great Linux dist!  Ubuntu is to Debian what
Mandrake was to RedHat in the beginning: RedHat + all the tweaks you
usually had to apply.

I remember the first install, it was a Breeze!  All features and extras
of the laptop was of course not working.  After some digging I found a
few pages detailing what to do to get things like susped, hibernate and
such working.  A couple of hours later everything was working!  Cool,
that had to be the first computer where I actually bothered setting it
all up.

Now, two years later I run 7.10 and I am less than happy.  All upgrades
have made this laptop a complete mess.  Each upgrade has failed to
integrate the necessary changes I made and simply installed the new
default settings from each package.  OK, I can live with that.  But I was
starting to really miss having no suspend, so the last four months I
have been working on fixing it all again.

It turns out that if I run the script `/etc/acpi/suspend.sh` manually
from the prompt suspend and resume works fine, even with X.  But when I
press Fn-F4 from within X or select "Suspend" from the Gnome "logout"
menu the machine happily goes to sleep but always fails at resume.  I
always get a black screen!

Why is it so complicated?  I really don't understand.  What is it doing?

OK, so I started digging.  The Fn-F4 button didn't work in the console
so that seemed like a first thing to investigate.  I started
`acpi_listen` and pressed Fn-F4, bingo!  It returned a key press.  A
grep later and I had found it in `/etc/acpi/events/ibm-sleepbtn`,
alright!  Simple syntax, seems it looks for the ThinkPad sleep button
and then sends another new combination `$KEY_SLEEP`, wtf?

I changed the action to call `/etc/acpi/sleep.sh`, restarted the ACPI
stuff and, yes it works!  At least from the console, not from X/Gnome.
Aha, so we have a Gnome app that does magic things, HAL +
gnome-power-manager.

I didn't get any further.  Removing gnome-power-manager fixed my
problem, I can now press Fn-F4 from Gnome, close the lid, come back the
next morning and sucessfully resume when opening the lid again.

So, the question is, wtf is gnome-power-manager doing and why do I have
to learn this stuff all over again every time I upgrade?
