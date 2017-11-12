---
layout: post
title: Assembly Language Debugging with gdb
tags:
- Assembly
- Decompilation
date: 2016-12-16
---

## Useful commands

- Run gdb in Text User Interface (TUI) mode
```shell
gdb -tui <program>
```

- Change the layout to Assembly
```
layout asm
```
- Or split the layout to C and Assembly
```
layout split
```
- Apply the following customization
```
set disassembly-flavor att/intel
set print asm-demangle
set disassemble-next-line on  //ask gdb to show us the next instruction every time
```
- Puts break point on main and invoke run
```
start
```
- Print the next N instructions that $pc or $rip points to.
```
x/Ni $pc
```
- Navigation and Print
  - Use `nexti,stepi` instead of `next, step` which traverse the source lines
  - `x/[NUM][SIZE][FORMAT]` is for examining memory; x $rax does not make sense
  - info frame/locals/registers
