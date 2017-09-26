---
layout: post
title: Day fourty-one
date: 2016-06-01 16:40:00 +1000
---

Java 8 Streams
-----

I've been looking into streams following my IPM, in the hope or reducing the amount of loops in my Tic Tac Toe code needed to check, for example, if my board is full and if it is empty, and I thought that my research might make a good blog post.

### What are Streams?

The Java docs define a stream as "A sequence of elements supporting sequential and parallel aggregate operations".  I think this brings up three main points.

1.  A stream is a sequence of elements, not a collection of elements.  Java Streams do not store elements like a collection does, instead it computes them on demand.
2.  Streams can be parallel or sequential
3.  Streams support aggregate operations so can process the sequence of elements as a whole.

Two extra key features of streams are:

1.  Pipelining:  some streams return streams, so can be chained.
2.  Internal Iteration: streams are iterating behind the scenes rather than explicitly defining the iteration.

This illustrates one of the key differences between collections and streams.  If we wanted to change the elements of an array, for example by squaring them and then sorting them, we would have to save the array of squared numbers before we could sort it.  Streams, however, because they iterate internally, keep track of the mutated data.


```java
public static List<Integer> sortedSquares(List<Integer> numbers) {
  List<Integer> squares = new ArrayList<>();
  for (int num : numbers) {
    squares.add(num * num);
  }
  Collections.sort(squares);
  return squares;
}
```
Streams save the changed arrays, and can call all chained functions on the mutated array:

```java
public static List<Integer> streamSortedSquares(List<Integer> numbers) {
  List<Integer> squares = numbers.stream()
    .map(num -> num * num)
    .sorted()
    .collect(Collectors.toList());
  return squares;
}
```

### Streams v Collections

There are other key differences between streams and collections.  The main one lies in when things are computed.

* Collections are a memory data structure.  They hold all of the elements in the collection to allow easy access to each element, but this means every elements has to be computed before it can be added to the collection.

* Streams are not concern with direct access to elements, they work with aggregate operations.  Streams thus allow the declarative description of the source, which could be an array, a file or any other collection, and the operations to take place on that source.  So a stream is conceptually fixed, and the elements within it are created on demand.

### Types of Stream Operations

There are two types of stream operations:

* Intermediate operations

These operations return streams, and so can be chained together.  These are also lazy, so they do not perform any processing until a terminal operation is invoked.

* Terminal operations

These are the operations like .collect() which return something other than a stream, so ending the chain of stream operations.

### Why use streams?

So far the key benefit I've seen from streams is that they reduce the amount of code you have to write and save you from having to write mutliple loops.  Interestingly, I've noticed that for small sets of data, they are not as fast as loops.  In the above examples where I passed in 10 numbers, the streamed method took around 150ms whereas the looped method took less than one.  With 1000 numbers the looped method went up to 1ms, and the streamed method stayed more or less the same.

I haven't yet had a reason to use streams with a larger set of data, but it would be interesting to see how the speed compares as the data set grows.

Java 8 also allows the easy use of parallel streams: 

```java
public static List<Integer> streamSortedSquares(List<Integer> numbers) {
  List<Integer> squares = numbers.parallelStream()
    .map(num -> num * num)
    .sorted()
    .collect(Collectors.toList());
  return squares;
}
```

Which, again, I'm sure would be useful for large sets of data, but it seems for a smaller set the overhead of dividing the data between the streams is not worth the time it takes for the data to be processed by the streams.  For 1000 numbers passed in, the parallel stream took around 20ms longer than the single stream.

So those are some thoughts about streams.  There is a lot more to them, and trying to make the most of lazy operations and short-circuiting that streams provide would be really fun, but I think it's probably out of scope for my Tic Tac Toe.
