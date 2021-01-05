---
title: GDB
category: C-like
tags: [WIP]
updated: 2021-01-05
weight: -2
---

Intro
-------------------------------------

- `run [command-line arguments]` runs the program
- `quit` quits gdb
{: .-prime}

### Breakpoints

- `break <function_name | line_number | file:line_number>`
- `delete <breakpoint number>`
- `next` instruction without diving into functions
- `step` into a function
- `continue`
- `finish` continue until current function returns

### Stack backtrace

- `bt` backtrace
- `print [variable]` prints the value of the variable
- `info locals` prints the values of all local variables
- `info args` prints arguments of the current function
- `info breakpoints` prints info about breakpoints

