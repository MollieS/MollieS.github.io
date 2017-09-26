---
layout: post
title: Day fourty nine
date: 2016-06-11 10:30:00 +1000
---

# Big O and Logarithms
--------

It took me a long time to get my head around Logarithmic time, and I just want to make a small note about why I found it hard.  Logarithms, for me, was a difficult concept. I had never heard of the term before leaning abut it in relation to Big O, so it was never going to be that easy to comprehend, but I believe that most of the articles I read were aimed towards people with a lot more knowledge than I had.  It seemed as if the articles were written to explain logarithms and Big O to people who already had a deep understanding of logarithms in maths.  When I tried to reserch Logarithms outside of Big O so that I could understand them in relation to Big O, it seemed as if all the articles explaining Logarithms were directed towards people who knew a lot about maths.

Maybe I should have expected this, but actually, know that I understand a little more abut the concept, I don't think it is as complicated as I did at first.  The base level of understanding the articles I was reading assumed made it seem incredibly complicated, and I often found the language they used to be a little intimidating which only added to the feeling that this was a concept that was way above my understanding.  The first line of the wikipedia article on Logarithms is this:

*In mathmatics, the logarithm is the inverse operation to exponeniation*

Whatd does that even mean?  The first time I read this it honestly seemed like a string of words connecting together with no meaning.  Because I just had no idea what it was saying.  So again, I'm going to try and explain the logarithm in a way I can understand now, and would have understood then.

### What is the Logarithm in relation to Big O?
-----

Exponeniation is simply referring to exponential growth, for how an algorithm with O(n2) notation grows in proportion to the amount of data it processes.  Exponential growth starts small, the time taken to complete an operation is quite small, and then grows faster than the size data structure. So it growth is not proportional to the number of elements. If we compare it to linear growth, O(n), exponential growth begins taking less time than a linear algorithm, but as the data size increases, it takes more time than a linear alogorithm, and as the sata size continues to grow, an exponential alrogithm will take exponentially more time than a linear one.

Logarithm is the opposite of this.  Growth rapidly goes up, and then steadies.  It is useful for large data sets as it is quite high for a smaller data structure, but as that data struture grows, the growth stabalises.  So, in comparison with linear growth, to begin with a logarithmic algorithm would take more time than a linear one, but as the data structure grows then the logarithmic algorithm begins to take less time than the linear algorithm.  It, like exponential time, has growth not proportional to the rate at which the number of elements grows, but as exponential growht grows proportionally faster, logarithmic growth is proportoinally slower.

So logarithmic is simply another term to denote the rate of growth in relation to Big O.  This is not a complete description of what the logarith is, but I think for Big O notation, this is enough.

### O(log n)
-------

Binary search is the most common example of an O(log n) algorithm. Logarithmic runtime is acheived when an algorithm has many possible elements to choose from when making an action, but only one needs to be chosen.  So we have a O(n) find algorithm:

```java
public Integer find(Integer[] array, Integer elementToBeFound) {
  for (Integer element : array) {
    if (element == elementToBeFound) {
      return element;
    }
    return null;
  }
}
```

Binary search is more efficient since it does not have to check every element.  It can choose which elements to check, and which to ignore.  Binary search only works on a sorted array, and can be implemented as follows:

```java
public Integer search(Integer[] array, int elementToFind) {
  int max = array.length -1;
  int min = 0;
  int middle;
  while (min <= max) {
    count ++;
    middle = (min + max) /2;
    if (array[middle] > elementToFind) {
      max = middle - 1;
    } else if (array[middle] < elementToFind) {
      min = middle + 1;
    } else {
      return array[middle];
    }
  }
  return null;
}
```

If we compare these two algorithms for a sorted array with five elements `Integer[] array = {1, 2, 3, 4, 5}`  the `find(5)` method would have to take five, n, steps in order to find five.  The binary search only has to take 2.  For a 1000 element array, to find(998) will take 998 steps, but for a binary search it will take only 9.  So we can see that for a small array the difference between the linear method and the logarithmic method are quite small, but as the array becomes larger the difference is obvious.
