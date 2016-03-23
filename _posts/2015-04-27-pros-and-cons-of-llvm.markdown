---
layout: post
title: "Pros and Cons of LLVM"
date: 2015-04-27 22:18:38 -0400
comments: true
published: false
categories: compilers, llvm, analysis
---
For many of my past projects, I've used [LLVM](<http://llvm.org/>) a compiler
framework. At the highest level, LLVM is an [intermediate
language](<https://en.wikipedia.org/wiki/Intermediate_language>) (LLVM IR)
along with an extensive library of functions capable of transforming and
examining the language. As such, it is comparable to GCC's
[GIMPLE](<https://gcc.gnu.org/onlinedocs/gccint/GIMPLE.html>),
[C--](<https://en.wikipedia.org/wiki/C-->), or
[CIL](<https://kerneis.github.io/cil/>). All of these intermediate languages
share a common goal: to ease the analysis and transformation of a program.

In this post, I share some of the quirks of LLVM which may be good or bad
depending on your situation.
