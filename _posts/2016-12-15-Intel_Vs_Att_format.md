---
layout: post
title: Intel vs AT&T Syntax
tags: 
- Assembly
- Decompilation
date: 2016-12-15 
---

## Prefixes

- In Intel syntax there are no register prefixes or immed prefixes. 
- In AT&T however registers are prefixed with a '%' and immed's are prefixed with a '$'. 


{% highlight assembly %}


   Intel Syntax
   mov     eax,1
   mov     ebx,0ffh
   int     80h

   AT&T Syntax
   movl    $1,%eax
   movl    $0xff,%ebx
   int     $0x80
{% endhighlight %}


## Direction of Operands.
- Intel: `instr dest src`
- Att: `instr srd dest`

``` 
 Intel Syntax
 instr   dest,source
 mov     eax,[ecx]

 AT&T Syntax
 instr   source,dest
 movl    (%ecx),%eax
```

## Memory Operands.

- In Intel syntax the base register is enclosed in '[' and ']' 
- In AT&T syntax it is enclosed in '(' and ')'.

```
Intex Syntax
mov     eax,[ebx]
mov     eax,[ebx+3]

AT&T Syntax
movl    (%ebx),%eax
movl    3(%ebx),%eax 
```

## Complex instructions
- Intel: `segreg:[base+index*scale+disp]` 
- Att:  `%segreg:disp(base,index,scale) == disp + base + index*scale`, where scale defaults to 1 if absent.

```
Intel Syntax
instr   foo,segreg:[base+index*scale+disp]
mov     eax,[ebx+20h]
add     eax,[ebx+ecx*2h
lea     eax,[ebx+ecx]
sub     eax,[ebx+ecx*4h-20h]

AT&T Syntax
instr   %segreg:disp(base,index,scale),foo
movl    0x20(%ebx),%eax
addl    (%ebx,%ecx,0x2),%eax
leal    (%ebx,%ecx),%eax
subl    -0x20(%ebx,%ecx,0x4),%eax
```

## Suffixes

- Att: The AT&T syntax **mnemonics** have a suffix. The significance of this suffix is that of operand size. 'l' is for long(32), 'w' is for word, and 'b' is for byte. 
- Intel: Suffix for **memory operands**, i.e. byte ptr(8), word ptr(16), dword ptr(32). 
  
  ```
   Intel Syntax
   mov     al,bl
   mov     ax,bx
   mov     eax,ebx
   mov     eax, dword ptr [ebx]
  
   AT&T Syntax
   movb    %bl,%al
   movw    %bx,%ax
   movl    %ebx,%eax
   movl    (%ebx),%eax
  ```
