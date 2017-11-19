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

- Examine memory: x/FMT ADDRESS.
  - ADDRESS is an expression for the memory address to examine.
  - FMT == [NUM][SIZE][FORMAT]
    - [Format] ::= [ o(octal), x(hex), d(decimal), u(unsigned decimal), t(binary), f(float), a(address), i(instruction), c(char), s(string), z(hex, zero padded on the left)].
    - [Size] ::= [b(byte), h(halfword), w(word), g(giant, 8 bytes)]
    - The specified number of objects of the specified size are printed according to the format.  If a negative number is specified, memory is examined backward from the address.
    - Example: Print the next N instructions that $pc or $rip points to.
      ```
      x/Ni $pc
      ```
- Use `nexti,stepi` instead of `next, step` which traverse the source lines
