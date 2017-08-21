---
layout: post
title: Assembly Language Debugging with gdb
tags: 
- Assembly
- Decompilation
date: 2016-12-16
---

## Useful commands
 - gdb -tui <program>
   - layout asm
   - start //puts break point on main and invoke run
   - x/i $pc // Print the instruction that $pc or $rip points to. ($pc is arch independent name for program counter)
   - set disassemble-next-line on //ask gdb to show us the next instrcution every time
   - nexti,stepi // instead of next, step which traverse the source lines
  - x/[NUM][SIZE][FORMAT] is for examining memory; x $rax does  not make sense
  - With p/x $rax, it does not makes sense to provide SIZE as the resgister type of $rax imples that
