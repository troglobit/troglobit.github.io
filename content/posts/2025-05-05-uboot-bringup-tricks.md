---
title: U-Boot Bring-up Tricks
date: 2025-05-05T13:57:00Z
tags: [ u-boot, buildroot ]
---

In my *Reminder to Self* series, this week we turn to some tricks I
learned while porting U-Boot to a new board.  First, a few tools:

 - `vbindiff`: Linux tool to examine, e.g., `flash.bin`
 - `binwalk`: Linux tool to examine an unknown binary file
 - `bdinfo`: U-Boot command to list board info

When poking through such binaries, the FDT (flattened device tree)
header magic to look for is `0xd00dfeed`.

Huge props to wkz for all the help and sparring, cheers!

### Boot Loader Stages

On Aarch64 the SoC goes through several stages to provide devices with
secure boot facilities.  Here's a brief overview:

 1. FPL, or Boot ROM, exists in various forms and versions
 2. SPL, second-stage program loader, usually a stripped-down U-Boot
 3. TPL, third stage program loader
    - BL31, usually ATF
	- BL32, optional, usually OP-TEE today
	- BL33, usually U-Boot proper

Always try to get a copy of the SoC reference manual.  Most vendors
provide one, as long as you sign up and offer up your firstborn.  Here's
a way in for the NXP i.MX8MP:

 - https://www.nxp.com/docs/en/preview/PREVIEW_IMX8MPRM.pdf

Read up on how your specific SoC vendor wants the blobs lined up on
flash to load.  They all have different ways of doing it.


### First Trick!

The first trick is perhaps the most obvious one: start out from an
existing board for the same SoC.  Vendors usually provide reference
boards that come with reference U-Boot and Linux implementations.  If
you're unlucky, the vendor may not have upstreamed that support to
mainline yet.

The best advice I've found is in this step-by-step guide by Quentin
Schulz, a Bootlin engineer:

 - https://bootlin.com/pub/conferences/2017/elce/schulz-how-to-support-new-board-u-boot-linux/schulz-how-to-support-new-board-u-boot-linux.pdf

On page four:

 1. Have some code to validate that the IP block you're working on works
 2. Have reference code
 3. Have code you can use to debug

Focus on getting just RAM and UART configured correctly, commit, move on
to the next IP block, commit, and repeat.


### Did I Get Here?

Another trick wkz showed me — the same as item 3 above — was to write
directly to the UART FIFO at various points, to see how far you've
gotten.

I got the following code from wkz to figure out which UART I was on:

```C
__attribute__((naked)) void _helo(void)
{
    volatile unsigned int *u1 = (unsigned int *)0x30860040;
    volatile unsigned int *u2 = (unsigned int *)0x30890040;
    volatile unsigned int *u3 = (unsigned int *)0x30880040;

    while (1) {
        *u1 = '1';
        *u2 = '2';
        *u3 = '3';
    }
}
```

He compiled this to a binary he could jump to early on, and even call
directly from early assembly code.  Very useful!

Here's what I ended up using for the i.MX8MP:

```C
#if 1
#define UART_BASE    (unsigned int *)0x30860000
#define UART_TXD     (UART_BASE + 0x40)
#define UART_UTS     (UART_BASE + 0xb4)
#define UART_TXFULL  (1 << 4)

void putta(char *str)
{
    volatile unsigned int *uts = (unsigned int *)0x308600b4;
    volatile unsigned int *txd = (unsigned int *)0x30860040;

    while (*str) {
	    while (readl(uts) & UART_TXFULL)
		    schedule();

	    writel(*str, txd);
	    str++;
    }
}
#else
#define putta(str)
#endif
```

This provides a simple `puts()` implementation for UART1 that can be
used from SPL, ATF and U-Boot proper.


### U-Boot Specific Tricks

All `debug()` statements in U-Boot code can be enabled by adding the
following line to the beginning of a C file, before all includes:

    #define DEBUG 1

To dump memory from C code:

    #include <hexdump.h>
    
    printf("Hex dump of 0x%lx:\n", spl_image.entry_point);
    print_hex_dump_bytes("", DUMP_PREFIX_NONE, (void *)spl_image.entry_point, 64);
