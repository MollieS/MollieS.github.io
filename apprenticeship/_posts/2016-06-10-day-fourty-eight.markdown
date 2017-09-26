---
layout: post
title: Day fourty-eight
date: 2016-06-10 17:40:00 +1000
---

Big O Notation
-------

Big O notation denotes the efficiency of an algorithm, and how it is affected by the number of elements.  It calculates the number of steps it takes for an algorithm to complete the task and, more importantly, how the algorithms scale when the data structure grows.

It took me a while to really get to grips with the concept, which is why in this post I'm going to include two examples of some of the O notations that I struggled with.  I think that when I write a blog post like this which can, hopefully, help someone understand something, I try to write it as the blog post that I wish I had read when learning about it.

### Constant Time
------

A constant time operation is usually the most efficient type, especially if it is O(1).  If it is constant time then, no matter the size of the data structure it acts upon, it will take the same number of steps and therefore the same time to complete its action.  

For example, accessing a specific element in an array will always be O(1) since you already know where the element is located.

```java
public int getFirst(Integer[] array) {
  return array[0];
}
```

Not all constant time operations are O(1) though.  Constant time refers to any algorithm that takes the same number of steps regardless of the size of the data structure, but can be O(1), O(2), O(3) and so on.  This means having a constant time operation is not always the optimal solution.  If you had O(1000) and a data structure that was only 20 elements long, this is obviously not the best choice.  Generally speaking, though, constant time is a good thing since it isn't related to the size of the data structure and thus scales very easily.

### O(n)
------

For an O(n) operation the number of steps it has to take in order to complete its task is directly linked to the amount of elements in the array.  In general, when talking about Big O notation, the n will always refer to the number of elements in the data structure. An O(n) operation is also sometimes referred to as a linear time operation as the steps taken grows at the same rate as the growth of the data structure.

```java
public Integer size(Integer[] array) {
  int count = 0;
  for (Integer element : array) {
    count++;
  }
  return count;
}
```

This will always be O(n) because we have to go through each element of the array in order to count it.

The classic example of an O(n) algorithm is finding the largest or smallest element in a collection:

```java
public Integer findSmallest(array) {
  Integer smallest = null;
  for (int counter = 0; counter < array.length; counter++) {
    if (smallest == null || smallest > array[counter]) {
      smallest = array[counter];
    }
  }
  return smallest;
}
```

This algorithm has to go through each element of the array, because you cannot be sure if the current smallest element is the actual smallest element until you have checked every element.

Big O will, however, always favour the worst case scenario, also known as the upperbound, so if we were to have this method:

```java
public Integer find(int element) {
  for (Integer number : array) {
    if (number == element) {
      return number;
    }
  }
  return null;
}
```

And we had this array:

```java
Integer[] array = {1, 2, 3, 4, 5};
```

This particular operation would be only need to complete one step, but, if we wanted to call `array.find(5);` it would be O(n).  Similarly, if we called `array.find(6);` it would be O(n) as we would have to go through each element to check that the array did not contain 6.  So this find method would be categorised as O(n) even though calling it may take one operation, the worst case is having to iterate through all elements.

### O(2n) 
-------
For O(2n) the number of steps doubles for every addition to the data structure.  The classic example of an O(2n) function is the recursive calculation of the Fibonacci sequence.  O(2n) is also linear time, as its performance is directly related to the elements in the data structure in the same way as O(n), just doubled.

```java
public int Fibonacci(int number) {
  if (number <= 1) {
    return number;
  }
  return Fibonacci(number - 2) + Fibonacci(number - 1);
}
```

The number of steps doubles for the amount of numbers calculated because you have to recursively calculate the number twice.

### O(n²) 
-----

O(n²) states that the perfomance is directly proportional to the square of the number of elements, so for every element added, we will have to complete the square of the number of elements.  O(n²) is ususally classes as exponential time as the steps taken to complete the task start off relatively small when n is small, but as the size of n increases, the time taken to complete the task increases exponentially.

O(n²) is common with nested loops:

```java
public boolean containsAPair() {
  for (int indexOfElement = 0; indexOfElement < array.size(); indexOfElement++) {
    for (int indexOfPossiblePair = 0; indexOfPossiblePair < array.size(); indexOfPossiblePair++) {
      if (array.get(indexOfElement) == array.get(indexOfPossiblePair)) {
        return true;
      }
    }
  }
  return false;
}
```

We can see that for however many elements you have, n, you will have to check n * n in order to compare each element to the other elements in the array to ensure whether or not there is a pair. 

The classic O(n²) example is the bubble sort algorithm:

```java
public Integer[] bubbleSort(Integer[] array) {
  int arraySize = array.length;
  while (true) {
    boolean swapped = false;
    for (int counter = 1; counter < arraySize; counter++) {
      if (array[counter] < array[counter - 1]) {
        int toBeSwapped = array[counter];
        array[counter] = array[counter - 1];
        array[counter - 1] = toBeSwapped;
        swapped = true;
      }
    }
    if (!swapped) { break; }
  }
  return array;
}
```

For a sorted array, the best case scenario, the algorithm still has to iterate through all elements of the array in oreder to check that it is sorted.  For a reverse ordered array, the worst case scenario, the algorithm has check n - 1 elements to check to complete the first round of swaps , and then go through the process n times.  So, if we passed in:

```java
Integer[] array = {5, 4, 3, 2, 1};
```

After the first while loop completes, the array would look like this:
```{4, 3, 2, 1, 5}```
After the second:
```{3, 2, 1, 4, 5}```
After the third:
```{2, 1, 3, 4, 5}```
After the fourth:
```{1, 2, 3, 4, 5}```

And, even though the array is now sorted, we have to iterate over it one more time to check that no swaps need to be completed, so after the fifth/nth iteration we break the while loop and return the sorted array.  Note also that because there is no memoization or optimization of the algorithm, while iterating through the array, the algorithm will continue to check each element even when that element has already been sorted.

Although BubbleSort is a classic example of an O(n²) it is not exactly O(n²).  The number of steps this implementation of BubbleSort takes to complete is actually O(n² - n), but Big O notation is not exact.  There are many other factors that will affect the efficiency of an algorithm, and because Big O favours the worst case, it will in general, round up.

So that's an overview of basic Big O Notation and my next post will tackle logarithms and Big O.

