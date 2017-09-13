---
layout: post
title:  Leveraging Undefined Behavious
tags: 
- Undefined behaviour
date: 2017-09-12 
---

# Leveraging undefined behavious by optimizing compilers.
## Case I
```C
#include <iostream>

int fermat() {
  const int MAX = 1000;
  int a=1,b=1,c=1;
  // Endless loop with no side effects is UB
  while (1) {
    if (((a*a*a) == ((b*b*b)+(c*c*c)))) return 1;
    a++;
    if (a>MAX) { a=1; b++; }
    if (b>MAX) { b=1; c++; }
    if (c>MAX) { c=1;}
  }
  return 0;
}

int main() {
  if (fermat())
    std::cout << "Fermat's Last Theorem has been disproved.\n";
  else
    std::cout << "Fermat's Last Theorem has not been disproved.\n";
}
```
Result:
```
Fermat's Last Theorem has been disproved.
```

Despite the fact that this program does not contain any arithmetic overflows
(multiplier factors vary in the range from 1 to 1000, the sum of their cubes
 does not exceed 2^31), the C++ standard defines an infinite loop as an
undefined action, without changing the external state. That’s why C++ compilers
are entitled to consider similar loops as finite.

The compiler can easily see that the only way out of the while(1) loop is the
return 1; statement, while the return 0; statement at the end of fermat()
cannot be reached. Therefore, it optimizes this function to

int fermat (void)
{
  return 1;
}
In other words, the only possibility to write an infinite loop that could not be
removed by the compiler is to add a modification of the external state to the
loop body.

## Case II
```C
int table[4];
bool exists_in_table(int v)
{
    for (int i = 0; i <= 4; i++) {
        if (table[i] == v) return true;
    }
    return false;
}
```

First of all, you might notice the off-by-one error in the loop control. The
result is that the function reads one past the end of the table array before
giving up. A classical compiler wouldn't particularly care. It would just
generate the code to read the out-of-bounds array element (despite the fact
    that doing so is a violation of the language rules), and it would return
true if the memory one past the end of the array happened to match.

A post-classical compiler, on the other hand, might perform the following
analysis:

- The first four times through the loop, the function might return true.
- When i is 4, the code performs undefined behavior. Since undefined behavior
lets me do anything I want, I can totally ignore that case and proceed on the
assumption that i is never 4. (If the assumption is violated, then something
    unpredictable happens, but that's okay, because undefined behavior grants
    me permission to be unpredictable.)
- The case where i is 5 never occurs, because in order to get there, I first
have to get through the case where i is 4, which I have already assumed cannot
happen.
- Therefore, all legal code paths return true.  As a result, a post-classical
compiler can optimize the function to ```C bool exists_in_table(int v) { return
  true; } ```

## Case III
```C
int foo(int x) {
    return x+1 > x; // either true or UB due to signed overflow
}
```
may be compiled as (demo)
```C
foo(int):
        movl    $1, %eax
        ret
```
Because in all legal cases true is returned.

## Case IV
```C
std::size_t f(int x)
{
    std::size_t a;
    if(x) // either x nonzero or UB
        a = 42;
    return a;
}
```
May be compiled as (demo)
```C
f(int):
        mov     eax, 42
        ret
```
Because in all legal cases 42 is returned.

```C
bool p; // uninitialized local variable
if(p) // UB access to uninitialized scalar
    std::puts("p is true");
if(!p) // UB access to uninitialized scalar
    std::puts("p is false");
```
Possible output:
```
p is true
p is false
```
The code will ub are optimized out.

## Case V
```C
int foo(int* p) {
    int x = *p;
    if(!p) return x; // Either UB above or this branch is never taken
    else return 0;
}
```
may be compiled as
```
foo(int*):
        xorl    %eax, %eax
        ret

```
If p is null, the if access is UD; If p is non null, then `return 0` will
happen. So in legal cases, 0 is returned and so is the optimization.
