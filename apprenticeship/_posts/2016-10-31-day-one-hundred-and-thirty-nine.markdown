---
layout: post
title: Day one-hundred and thirty-nine
date: 2016-10-31
---

RAM
---------------------------------

### What is RAM?
----------------

Computers are made up of chips.  There are two main kind of chips:

1. Combinational
2. Sequential

The chips that compute boolean functions, logic and arithmetic, are combinational chips.  The output they give is based solely on the combination of input they are given.

Sequential chips are used to build memory chips and these make up the basics of Random Access Memory.

### Memory
----------------

In order for a computer to maintain state, it needs to address two issues:

1. How to store data
2. How to retrieve data

### How to Remember
--------------------

The concept of memory is inherently tied to the concept of time.  In order to remember anything, you need to be conscious that time has passed. So for a computer to be able to recall something, it needs a standard representation of the progression of time.  This is done with the use of a Clock. In most computers, time passing is represented by a master clock which delivers a contionous sequence of alternating signals.

The hardware of this clock is usually an oscillator that alternates between two phases.  These two phases are usually referred to as either 1 and 0 or tick and tock.  The time in between each tick and tock is a cycle, and each cycle is a unit of time.  Each phase, tick and tock, is represented by a binary signal which is transmitted to every sequential chip throughout the computer's platform using the hardware's circuit.

### The Building Blocks Of RAM
--------------------

As with combinational chips, the complex elements of a computer system can be built from elementry chips.  For sequential chips, these are types of Flip Flop gates.  A Data Flip Flop is a variation of a Flip Flop gate, and can be used to maintain state.

A DFF takes a single bit input and outbuts a single bit.  It has access to the clock, and it looks like this:


Having a data input and a time input allows the DFF to implement time based behaviour, out(t) = in(t-1).  In essence, what the DFF does is output the input value from the previous time unit.

This does not allow it to maintain state, though.  All a DFF has access to is the data from the previous cycle.  In order to store a value over time, we can use the DFF to create a Register.  In order to persist data, a register takes the output from a DFF and passes it back into the DFF, so that the data can be stored continuously.  In theory, then, a register could look like this:


But this is an invalid design for two reasons.  Firstly, loading the chip becomes impossible as there is no way to tell the chip what its input is.  Secondly, the rules of chip design state that a chip can only take input from a single source.  In order to create a working register, we need a Multiplexor

### MUX
--------------------

The Mux gate takes 3 sets of input.  It takes two regular inputs, a and b, and another input, the selector.  The selector determines which input will be outputted.  If the selector is 1, then the output will be a, if the selector is 0, then the output will be b. The gate looks like this:


### Register
---------------------------

A register with a MUX and DFF game would therefore look like this:


The MUX gate allows us to give the DFF the input that we want.  We can either give in new data by giving it a load value, or ask it to persist the data it currently has stored.

### Multi-Bit Registers
--------------------------

The register above will only deal with single bit input and output, but if we imagine our single bit register to look like this:


Then chaining multiple bit registers together to create a multi-bit register is simple:


And with a multi-bit register, we are a step closer to implementing RAM!


### Data Races
------------------

To have this circular design may seem dangerous.  Usually the output of a chip depends on the input, but the DFF have a time delay, which allows for a feedback loop like this.  In a combinational chip, the output would depend on the input, which would depend on the output, making the output depend on itself.  In a DFF, however, the output at time t does not depend on itself because it depends on the output at t - 1, so avoiding a data race.

### Synchronizing Architecture
------------------

This ability of sequential gates can be used to synchronize the overall computer architecture.  If we were to have an ALU, a chip that requires multiple inputs, and these inputs were dependent on multiple other processes, it is very likely that the inputs would arrive at the ALU chip at different times due to the physical constraints of a computer.  Electrical pulses could travel at different times due to distance, resistance, interference, random noise or a whole host of other possibilities.  If every gate depended on its inputs to arrive at the same time, then the system would be exceedingly fragile.  

Combinational chips, like the ALU, though, have no concept of time and thus will continually act on whatever data happens to get to its inputs.  The output will only stabalize when the correct inputs are recieved, and until then it will generate nonsensical rubbish.

How can a sequential chip correct this?  ALU chips will, because of how computers are built, always connect its output to a sequential chip.  If the master clock ensures that the length of a cycle is longer than the time it takes for a bit to travel the greatest distance in the circuit, then we can guarentee that by the time the sequential chip updates itself, it will be receiveing the correct input from the ALU.

### RAM
-----------------------

So we have covered how we can solve the first issue of storing data using elementary chips, and now we need to solve how to access the stored data. The term "Random Access Memory" refers to the ability for read and write operations to access randomly chosen words.  Access should not depend on the order in which data is stored and any data stored should be accessed in equal speed, regardless of the location.

RAM is a collection of multi bit registers, and each register can store words.  In order to access these words, each word in the n-register RAM is given a unique address which will be between 0 and n-1.  In addition to the collection of registers, RAM contains a logic design that will allow it to select the individual register by the address given.

The idea of an "address" is not explicit in the RAM design.  The registers themselves are not marked with an address, it is the direct access logic that implements the addressing.

So, a RAM device takes three inputs:

1. Data
2. Address
3. Load

The address specifies which register should be accessed in the current time unit.

RAM design has two key parameters:

1. Width - the width of each word it can store
2. Size - the number of words in the RAM.

So, the final implementation of RAM would look like this:

