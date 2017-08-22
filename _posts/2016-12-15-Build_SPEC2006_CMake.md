---
layout: post
title: Build SPEC2006 using LLVM's cmake infrastructure
tags: 
- LLVM
- CMake
- SPEC CPU 2006 Benchmark
date: 2017-08-21 
---

## Steps to follow

```shell
  $LLVM_SRC=points to the root of LLVM source code tree.
  $LLVM_BLD=points to the root of LLVM build  tree.
  $SPEC_SRC=points to the root of SPEC 2006 source code tree.
  TESTSUITE_BUILD_DIR= points to the build dir of test-suite

  mkdir $LLVM_SRC/projects/test-suite/test-suite-externals
  ln -s $SPEC_SRC $LLVM_SRC/projects/test-suite/test-suite-externals/speccpu2006

  mkdir $TESTSUITE_BUILD_DIR && cd $TESTSUITE_BUILD_DIR
  cmake $LLVM_SRC/projects/test-suite -DCMAKE_C_COMPILER=<c compiler> -DCMAKE_CXX_COMPILER=<c++ compiler>
  cd External/SPEC/CINT2006/
  make -j 8
  lit -v -j 8 . -o results.json
```
