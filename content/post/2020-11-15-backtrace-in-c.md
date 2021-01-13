---
title: "Backtrace in C"
subtitle: ""
date: 2020-11-15 17:25:00 +0100
tags: []
---

Reminder to self: remember `CFLAGS=-g -Og -rdynamic` to allow
`backtrace_symbols()` to actually pretty print function names
in stack traces.

<!--more-->

Full example:

```sh
jocke@luthien:~/src/pim6sd [master]$ ./configure CFLAGS="-g -Og -rdynamic" && make V=1 clean all
```

Source code for stack tracer to instrument your code with:

```C
#include <execinfo.h>

void print_trace(void)
{
    void *array[10];
    char **strings;
    int size, i;

    size = backtrace(array, 10);
    strings = backtrace_symbols(array, size);
    if (strings != NULL)
    {
        printf("Obtained %d stack frames.\n", size);
        for (i = 0; i < size; i++)
            printf("%s\n", strings[i]);
    }

    free(strings);
}
```

