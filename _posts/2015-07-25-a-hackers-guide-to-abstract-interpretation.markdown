---
layout: post
title: "A Hackers Guide to Abstract Interpretation"
date: 2015-07-25 11:38:08 -0400
comments: true
published: false
categories: program analysis, abstract interpretation
---

## Introduction

When I was just starting my study of program analysis, I attended the 2013 SRI
Summer School on Formal Methods. There, I met a PhD student from UC Berkeley.
He said "abstract interpretation was the hardest thing he ever studied." Of
course after hearing this I wanted to know what it was. Sadly, it was deemed
too complex for my brain and, ever since, I've given abstract interpretation a
mythical status. It is only for wizards who dream in lattices, pray every day
to Galois, and learned first-order logic as a second language at age nine.

In reality, building an abstract interpreter is fairly easy and intuitive. As
we can see from the name, you just need to create an interpreter for your
language. So, what is an interpreter? It is no different than an interpreter
for an interpreted language like Python. Consider the following program:

``` c 
x = 5;
y = 10;
z = x + y;
```

Here, we have three variables: `x`, `y`, and `z`. Our interpreter will need to
keep track of the value of each variable in the program. To do this, we'll use
a map from a variable name to its value. Let's call this map `M` and use `M(x)`
to return the value associated with variable `x`.

Next, let's see how our interpreter would handle the previous program. Lets
assume, for simplicity, that each variable is initially zero. So, at the start
of the program:

```
M(x) = 0
M(y) = 0
M(z) = 0
```

Our interpreter then executes each line one by one following the flow of the
program. After executing `x = 5`:

```
M(x) = 5
M(y) = 0
M(z) = 0
```

Then, `y = 10`:

```
M(x) = 5
M(y) = 10
M(z) = 0
```

Finally, `z = x + y`:

```
M(x) = 5
M(y) = 10
M(z) = 15
```

During the execution, we examined each statement, e.g., `x = 5`, and then used
the statement to take an input `M` before the statement and produces an output
`M` after the statement is executed. More formally, the interpretation of each
individual statement is done using a *transfer function*. The transfer
function for a statement `x = 5` is:

```
return M(x) = 5
```

Or, for the slightly more complicated addition statement `z = x + y`:

```
return M(z) = M(x) + M(y)
```

To reiterate, the transfer function all have the same type: `M -> M`. They take
an input map and produce an output map reflecting the execution of some
statement.

Finally, with this interpreter execution, we have a few things. We can see the
value of each variable at each program location (lines 1--3). We can use this
information to answer questions like, "is the value of `x` non-zero at line 2?"

### So What? 

At this point, you may be upset I just spent so much of your time explaining
something trivial: all of the information we learned about the previous program
could have been obtained by simply executing it. I introduced some topics like
transfer functions and memory states but all of this is familiar, though
perhaps not so formally, to any programmer: writing code modifies memory.

This previous description, however, serves as the basis on which an analysis
can be built. What I just described was a *concrete* interpreter for a C-like
language. The term concrete just means it is a one-to-one mapping with the
actual semantics of the programming language being analyzed. For example, in a
C-like language, the statement `x + y` adds two variables, each representing a
single value. In our concrete interpreter description, we did just this when
computing the value of `z`. In abstract interpretation lingo, the description
of a programing language directly matching the actual behavior is called the
*concrete semantics*. We will see shortly how the term concrete is used to
distinguish for the *abstract*.

### Why Concrete is Hard

Reasoning about a program's concrete semantics is difficult. Here are two
examples to convince you.

Consider the following program:

```c
if (c1) {
  ...
}
else { 
  ...
}
if (c2) {
  ...
}
else {
  ...
}
```

Here, we have two branches on the values `c1` and `c2` respectively. The first
branch can either take the `if` or `else` direction and similarly for the
second branch. Since there are two decisions for the first branch and two
decisions for the second branch there are four possible paths through the
program. In general, a program with `n` branches has `2^n` paths.

If we wanted to use a concrete interpreter to analyze all these paths, we would
have to run it at least four times. We would also need to supply program
inputs in order to execute the different branches. This laborious testing
process is standard, for example, writing function level unit tests to increase
code coverage. 

To extend this further, consider the following:

```c
while (true) {
  x++
}
```

This program has only one path but it is infinitely long. Executing it with the
previously described concrete interpreter would never terminate. 

This complexity problem is well known and comes up in phrases like "the halting
problem" (you cannot tell if a program terminates?) and Rice's Theorem (it is
impossible to determine non-trivial properties of a program).

### Abstraction is Your Friend

What if we could write an interpreter which simultaneously executes all paths
for all possible inputs? What if we could write an interpreter with guaranteed
termination even for infinite programs? Both of these guarantees are provided
by an abstract interpreter.

It would be dishonest to continue without a disclaimer: to be able to handle
infinite programs and simultaneously explore all possible paths we must
sacrifice something. Specifically, our interpreter becomes less precise. We
lose some information about the concrete execution of the program in order to
guarantee termination. In the next section, I will go over some examples to
make this clear. 

The remainder of this guide will first introduce an abstract interpreter
through examples. I will show that creating an abstract interpreter is exactly
the same as creating a concrete interpreter. In fact, we've already gone over
all the building blocks (transfer functions and memory states). 

Because it is well covered, and also perhaps scary, I will avoid going over the
mathematical details supporting abstract interpretation: we'll avoid the usual
discussion on Galois connections and all that jazz about latices. We'll instead
focus more on the details required for implementing an analysis. 

This stuff is important, though! The formalization of abstract interpretation
allows us to make bold claims that our analysis will always terminate. If you
are creating a new abstract analysis, it is essential to prove properties about
it and to do so, you need math. For more mathematical details, you can read
Neilson and Neilson's book Principles of Program Analysis. 

## The Interval Domain

Next, I'll introduce a simple abstract interpreter using the *interval domain*.
I'll first discuss what the interval domain is, and then give an example of
using it to analyze a program.

If you recall from the previous section I mentioned some stuff about an
abstract interpretation losing precision relative to actually running the
program (i.e., the concrete semantics). This process of moving from the
concrete to the abstract is called abstraction. Consider the following:

```c
int main(int argc, char *argv[]) {
  int c = atoi(argv[1])
  int x;
  if (c > 10) {
    x = 10;
  }
  else {
    x = 5;
  }
  return x;
}
```

Here, we have a program with one branch where the value of `x` is set to either
`10` or `5` depending on the branch direction. The branch is controlled by the
user's input to the program from `argv`. (Assume for simplicity that the user
always supplies some input.)

Using the concrete semantics of the program, the value of `x` in the `return`
statement may be either `10` or `5`. The value of `c`, since it comes from the
user, could be anything.

If we wanted to concretely test this program we would need to test all possible
inputs which would require `2^32` executions (assuming 32 bit integers).
Obviously this would take a while!

Lets see how the interval domain can help us tackle this problem.

First, the interval domain represents a variable in the program by its lower
and upper bounds. For example, if we say `x = [-10, 27]` this means that `x`
has a value contained within -10 and 27. How is this useful? Well, it allows us
to represent a huge number of values as a single *abstract* concept.

Continuing the example, we know the value of `c` can be any value which can fit
into an `int`. This would be: -2147483648, -2147483647, ..., 0, 1, 2, ..., 2147483647. 
We can instead represent all of this values as the single interval
[-2147483648, 2147483647]. 


