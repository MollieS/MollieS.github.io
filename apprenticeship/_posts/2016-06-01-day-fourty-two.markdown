---
layout: post
title: Day fourty-two
date: 2016-06-02 16:40:30 +1000
---

Race Conditions
-----

Race conditions came up today in our Zagaku and although I knew I had read about them when I was reading about Rust, I couldn't actually remember what they were, so I thought if I wrote a blog post about them, I would hopefully remember better or, at least, have somewhere to refer to if I forget again.

### What is a Race Condition

In short, a race condition occurs when the output of a program or method is reliant on the sequence or timing of uncontrollable events.  Most often, these uncontrollable events are threads or processes.

A critical race condition is when the race condition results in invalid exceptions and bugs, which occur when events do not happen in the order the programmer intended.
A non critical race condition is when the behaviour of the program is unanticipated or unexpected, but not necessarily broken.

Race conditions are typically a result of processess or threads depending on shared state.  The operations on these shared states are critical sections.  They must be mutually exclusive or they could corrupt the shared state.

### Data Races

A data race is a type of critical race condition that is caused by concurrent threads which read and write from a shared memory location, which can lead to undefined behaviour.  The Rust documentation defines a data race as follows:

* two or more threads concurrently accessing a location of memory
* one of them is a write
* one of them is unsynchronized

### Why is this bad?

Obviously, you do not want undefined behaviour in your program.  You want to be completley confident that it works as expected.

One of the key difficulties of data races is their difficulty to test.  There could be a hundred examples of your program working perfectly, but every once in a while the behaviour is unanticipated.  Trying to recreate the scenario where it fails, where the order of your processes or threads changes, is exceedingly difficult.

### How do we limit Race Conditions?

[Immutability](https://mollies.github.io/2016/05/25/day-thirty-seven.html) helps to minimize possible affects of race conditions, as if two threads were to rely on a shared object, if that object were immutable, any writes to the object would create a new object and any thread depending on the unchanged object would still have the unchanged object.

### Can we eliminate Race Conditions?

No! Pretty much everything we use to write programs is racy including hardware and operating systems.  Users can be racy.  Everything can be racy, but once we realise that, it's just a question of minimizing the possibilities for race conditions where possible, and provisioning for them.


