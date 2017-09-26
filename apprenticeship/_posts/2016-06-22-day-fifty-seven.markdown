---
layout: post
title: Day fifty-seven
date: 2016-06-22 19:40:00 +1000
---

## Bits, Bytes and Buses
---------

### Bit

A bit is a basic unit of information and is the smallest unit of data in a computer.  Bit is short for binary digit, and a bit can only ever have two values.  These values can be interpreted different things depending on how they are processed.  They can be true or false, positive or negative, on or off etc, but they are represented as binary: either 1 or 0.

Bits are stored by any device that exists in either of two possible states.  They can be two stable states of a flip flop circuit, an electrical switch, directions of magnetization or polarization or any other device that has two distinct states.

In computing, a bit is usually represented by an electrical voltage or current pulse, or the state of a flip flop circuit.

#### Words

Processors are often designed to process data groups into words of a given length of bits.

The length of the word is an important feature of the computer's architecture, as the length of the word represents the largest piece of data that can be transferred to and from working memory by a single operation.

Word sizes are usually 8, 16, 24, 32 or 64 bits long.

### Byte

A byte is historically used to represent the group of bits used to encode a single character of text.  It is the smallest addressable unit of memory in many computer architectures and generally consists of eight bits.

### Bus

Hardware is typically designed to operate on multi-bit arrays which are called buses.  Because a bit is too small to be addressed to memory, they can be stored in a byte or a bus.  Using a bus allows multiple bits to be stored, and also allows each bit in the bus to be accessed by bit wise operations.

So, were you to have a 32 bit computer, you would expect that computer to be able to complete an And function on two 32 bit buses.  So you would have 32 And gates which operated seperatly each on a pair of bits.  To encapsulate these, you would have a chip that consisted of the 32 gates, two 32 bit buses for input, and one 32 bus for output.
