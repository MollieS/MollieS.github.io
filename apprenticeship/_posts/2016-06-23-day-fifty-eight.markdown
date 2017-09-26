---
layout: post
title: Day fifty-eight
date: 2016-06-24 18:40:00 +1000
---

## Amortized Analysis
------

Big O notation will always favour the worst case scenario, but in some cases the worst case scenario is too severe.  It may be more accurate and useful to represent the total worst-case cost for a sequence of operations.

For example, consider we have a dynamic array, an array that does not have a fixed size and therefore can be added to infinitley.  To implement this we would:

1. create an array
2. add to the array until it is full
3. when full, create a new array double the size of the full array
4. copy each element from the full array into the bigger array

The worst case scenario here would be adding to an array that was full, and therefore having to create a new array and copy every element over.  But if we were to represent that operation as the run time of the algorithm, it would not be an accurate representation, since this process only happens when the array is full, and we can make many insertions to an array that will not require the process.  So using Big O notation, the majority of the time inserting into the array would be an O(1) operations, but it could be an O(n) operation if the array is full, and since Big O always favours the worse case scenario, it would be represented as O(n).

Amortized analysis allows us to see a more balenced view of how costly an algorithm like insert into a dynamic array is, by spreading the cost of the large operation, the creating and copying process, over the less costly operations, inserting into the array.

There are three main types of amoritized analyisis:

* Aggregate method
* Bankers method
* Physicists method

### The Aggregate Method
----------

The Aggregate Method is the simplest way to amortize the cost of an algorithm.  It simply divides the cost of a sequence of operations by the number of operations taken, and can be represented as:

Cost of n operations ÷ n 

Because of this every operation is given the same cost, regardless of the type, so in the example of inserting into a dynamic array, the cost of adding to an array that is not full is the same as inserting into a full array.

### The Bankers Method
----------

Also known as the Accounting Method, the Bankers Method assignes a token to each operation it undertakes, which can be used to pay for the larger operation.  The token that is assigned can be of greater value than the cost that the operation actually takes, leaving positive credit that can be saved to pay for an operation that may cost more.  When using the Bankers Method it is important to note that it must be impossible to go into debt.  The assigned tokens must be great enough to pay for any sequence of operations.

With the dynamic array example, we can assign a token with the cost of three to each operation.  So, if starting with an array with the size of 1, the following would happen:

```java
[e1]        // add element to array
[e1] << e2  // add another element to the array
[ ,  ]      // create new array
[e1, ]      // copy from the old array
[e1, e2]    // add new element
```

And we can see the cost of each operation:

```java
             // cost = 0
[e1]         // cost++
[e1] << e2   
[ ,  ]       // cost++
[e1, ]       // cost++
[e1, e2]     // cost++
             // cost = 4
```

So the intial insert cost only 1 operation, and adding another element cost 3.  If we were to amortize the cost using the Bankers method, we would do the following:

```java
             // cost = 0
[e1]         // cost = 1 ( +1 for insertion, 2 in credit )
[e1] << e2   
[ ,  ]       // cost = 2 ( +1 for creating new array, 1 in credit )
[1,  ]       // cost = 3 ( +1 for copying the first element, 0 in credit )
[1, 2]       // cost = 4 ( +1 for insertion, 2 in credit )
             // cost = 4
```

It becomes clear that amortzied analysis cannot change the actual cost of an operation, but it can help represent it more accurately.  Without amortization, inserting into a dynamic array would be considered an O(n) operation, but with the amortized cost we can see that it is in fact an O(1) operation.

### The Physicists Method
----------

Also known as the Potential Method, the Physicists Method seems quite similar to the Bankers Method.  It stores "potential" in the data structure which can be used to pay for other operations.  Like the banker's tokens, the potential should never be negative and should be able to afford any sequaence of operations.

In the Bankers Method the amount charged for each operation was the amortized cost for that type of operations, but in the Physicists Method, the amortized cost of an operation is equal to the actual cost plus the increase in potential for that operation.  So, when an operation has a cost less than the amortized cost, the potential increases. Conversley, when an operations has a higher cost, it can be paid for with the potential, decreasing the available potential, just as the Bankers Method pays for operations in tokens.

The difference between the two is that the Bankers Method focusses on the upfront cost of an operation, prepaying any costly future operations.  The Physicist Method focuses on the effect of a particular operation at a given time.
