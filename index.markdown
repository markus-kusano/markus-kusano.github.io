---
layout: page
title: ""
date: 2014-08-29 14:31
comments: false
sharing: true
footer: true
---

{:refdef: style="text-align: center;"}
<img src="images/suite.jpg" width="252">
{:refdef}

I started my PhD at Virginia Tech in the Fall of 2014. My advisor is Chao Wang.
My research interests are in automated formal-methods and program analysis,
particularly the analysis of concurrent systems.

## Publications 

_Flow-sensitive Composition of Thread-modular Abstract Interpretation_, Markus
Kusano, Chao Wang. 24th International Symposium on the Foundations
of Software Engineering (FSE). Seattle, Washington, 2016.

_Static DOM Event Dependency Analysis for Testing Web Applications_, Chungha
Sung, Markus Kusano, Nishant Sinha, Chao Wang. 24th International Symposium on
the Foundations of Software Engineering (FSE). Seattle, Washington, 2016.

_Assertion Guided Symbolic Execution of Multithreaded Programs_, Shengjian Guo,
Markus Kusano, Chao Wang, Zijiang Yang, Aarti Gupta. 23rd 
International Symposium on the Foundations of Software Engineering (FSE).
Bergamo, Italy, 2015 ([PDF](/papers/GuoKWYG15.pdf)).

_ConcBugAssist: Constraint Solving for Diagnosis and Repair of Concurrent
Bugs_, Sepideh Khoshnood, Markus Kusano, Chao Wang. 2015 International
Symposium on Software Testing and Analysis (ISSTA). Baltimore, Maryland, 2015 
([PDF](/papers/concbugassist_preprint.pdf)) ([Slides](/papers/concbugassist_slides.pdf)).

_Dynamic Partial Order Reduction for Relaxed Memory Models_, Naling Zhang,
Markus Kusano, Chao Wang. 36th Conference on Programming
Language Design and Implementation (PLDI). Portland, Oregon, 2015 ([PDF](/papers/ZhangKW15.pdf)) 
([Slides](/papers/rinspect_slides.pdf)).

_Dynamic Generation of Likely Invariants for Multithreaded Programs_, Markus
Kusano, Arijit Chattopadhyay, Chao Wang. 37th International Conference on
Software Engineering (ICSE). Florence, Italy, 2015 ([PDF](/papers/KusanoCW15.pdf))
([Slides](/papers/udon_slides.pdf)).

_Assertion Guided Abstraction: A Cooperative Optimization for Dynamic Partial
Order Reduction_, Markus Kusano and Chao Wang. 29th International
Conference on Automated Software Engineering (ASE). Västerås, Sweden, 2014 ([PDF](/papers/kusano_pdpor.pdf)) 
([Slides](/papers/pdpor_slides.pdf)).

_CCmutator: A Mutation Generator for Concurrency Constructs in Multithreaded
C/C++ Applications_, Markus Kusano and Chao Wang. 28th International
Conference on Automated Software Engineering (ASE). Palo Alto, CA, 2013
([PDF](/papers/CCmutator_ase2013_preprint.pdf)).

## Awards

[NSF Graduate Research Fellow](<https://www.nsfgrfp.org/>) (2016)

[Virginia Tech Bradley Fellow](<https://www.ece.vt.edu/graduate/awards.html>) (2014)

## Internships

### Summer 2016

Samsung Research America, Mountain View CA.

### Summer 2014 & 2015

Research Assistant: NEC Labs America, Princeton NJ. Autonomic Management Group.

Designed, implemented, and tested big data heuristics to reduce the search
space of system intrusion detection algorithms. Efficiently analyzed
terabyte sized graph databases to create concise reports of intrusion
points to the end-user. Advisor: Zhichun Li.

## Software

### SysTest

[SysTest](<https://github.com/markus-kusano/SysTest>) is a dynamic stateless
model checker for concurrent programs. It implements the [dynamic partial order reduction algorithm](<https://dl.acm.org/citation.cfm?id=1040315>). It performs
dynamic instrumentation using the LLVM compiler framework. The scheduler is
written in Haskell and supports POSIX threads.

### CCmutator

I maintain [CCmutator](https://github.com/markus-kusano/CCMutator), a
multithreaded mutant generator for C/C++ applications. See the following
[paper](https://github.com/markus-kusano/markus-kusano.github.io/raw/master/papers/CCmutator_ase2013_preprint.pdf)
for more details
