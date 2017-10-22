---
categories: backup
comments: true
date: 2016-03-24T22:39:00Z
title: Disaster Recovery
url: /2016/03/24/disaster-recovery/
aliases: /blog/2016/03/24/disaster-recovery/
---

Days like these inconspicuously start out just like any other day,
except on days like these you accidentally manage to erase `$HOME` and
have no real backup to rely on ... Maundy Thursday will forever be Black
Thursday for me, from now on.

Best thing your can do, after cursing at yourself constantly for a
couple of hours, is to:

1. Come up with a useful backup and *restore* strategy
2. Read up on undeletion tools for Ext4
3. Blog about it, naturally

BUT FIRST -- QUICK -- UNMOUNT OR POWER-OFF YOUR COMPUTER -- PULL OUT THE
BATTERY -- AND STEP AWAY FROM THE COMPUTER!  Must protect the partition
from being accidentally written to -- I completely fumbled this step, so
take heed young people!

<!--more-->

Now calm down and act like an engineer again.

There exist two neat tools, three if you count the more hard core
`debugfs`:

- [extundelete](http://extundelete.sourceforge.net/)
- [ext4magic](http://ext4magic.sourceforge.net/ext4magic_en.html)

Both have been around a while, and both are capable of restoring all
files.  See their respective home pages for details.

My system used LVM, of course, to make things a bit harder.  That's why
I'm documenting my steps here:

1. Remove disk from lappy
2. Prepare to connect using USB cradle to workstation, *prepare* don't
   do it yet!  I did the next few steps as root on my workstation,
   that's right, I didn't care anymore at this point.  The world may as
   well burn!
3. You haven't connected the disk yet, right stupid?  Most systems today
   have cool features like automount that would cause the journal to be
   replayed -- WE DO NOT WANT THAT OK? OK!

You need the lvm tools:

    apt-get install lvm2
    vgscan
    vgchange -ay ubuntu-vg
    lvs

The last command should list your logical volume group(s), in my case
labled `ubuntu-vg`.  To be able to run `extundelete` or `ext4magic` on
the partition you have to mount the partition, *read-only* needless to
say:

    mkdir /rescue
    mount -o ro /dev/ubuntu-vg/root /rescue

Now we try some fairy dust:

    ext4magic /dev/ubuntu-vg/root -r

OK, so let's see:

    ls -l RECOVERDIR/

The results between `ext4magic` and `extundelete` may vary, so try them
both out, test different options, and maybe even look at `debugfs`,
we're desperate after all.

    extundelete /dev/ubuntu-vg/root --restore-all
    ls -l RECOVERED_FILES

If there's a LOT of files you may have some luck narrowing down the
result using the `--after DATE` option ... where `DATE` is the number
of seconds since the UNX epoch:

    extundelete /dev/ubuntu-vg/root --restore-all --after 1458802800
    ls -l RECOVERED_FILES

I found my magic number using

    date -d "Mar 24 8:00 2016" +%s

Well, as I said, I didn't turn off my computer in time. Instead I took
the braindead option of starting to google for solutions.  So all my
files (with proper filenames) turned out to contain only cached files
from the browser -- reclaiming the blocks goes quick, so watch out kids.
