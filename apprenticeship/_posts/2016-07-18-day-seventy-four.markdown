---
layout: post
title: Day seventy four
date: 2016-07-18 17:00:00 +1000
---

Priority Queues
------------

A Queue has two main algorithms assosciated with it:

1.  `PushBack(e)` or `Enqueue(e)` - adds an element to the back of the queue
2.  `PopFront()` or `Dequeue()` - gets the first element from the queue

Because a queue works on a FIFO, first in, first out, basis, a user always know what behaviour to expect.  A priority queue works in a similar way, but the order in which elements are returned is not preset and can be defined by the user.  The algorithms for a Priority Queue are:

1. `Insert(e)` - add an element to its correct place in the queue
2. `ExtractMax()` - gets the highest priority element
3. `ChangePriority(e)` - changes the priority of an element and reassigns it to its correct position
4. `Remove(e)` - remove an element from the queue

### Abstract Data Types
-------------

Queues and Priority Queues are Abstract Data Types.  This means that they are defined not by their implementation, but instead by the behaviour from the users perspective.  This is why algorithms are so important to Abstract Data Types, they are what differentiates one data structure from another.  Because an Abstract Data Structure is defined by its behaviour, its implementation is not defined and can be considered.


### Implementation
-------------

How is it best to store a priority queue?  We have a few options which can be discussed in terms of the two key algorithms of Priority Queues: Insert and ExtractMax.

1. Unsorted Array
2. Sorted Array
3. Sorted List
4. Binary Heap

#### Unsorted Array

Insert(e) is a very easy and cheap operation in an unsorted array.  It will always be O(1) as it does not matter where an element is inserted, so a new element is simply inserted to the end of the array.

ExtractMax() is where we pay the price for such a cheap insert method.  For an unsorted array, getting the maximum value is essentially a find operation, so will be O(n)

#### Sorted Array

For a sorted array, it is the Insert(e) method that becomes expensive.  We have to find where an element belongs in the array in order to place it.  So insert into a sorted array is O(n).

Because the array is sorted, the ExtractMax() method becomes cheap, O(1), but because of the added to cost to Insert, using a sorted array is no better than unsorted for a Priority Queue.

#### Sorted List

Lists are similar in behaviour to arrays, and for this reason implementing a Priority Queue with a Sorted List would have the same time complexity as a Sorted Array.

#### Binary Heap

A Binary Heap is the best way to implement a Priority Queue.  The Insert algorith is O(log n) and the ExtractMax algorithm is also O(log n).

### Binary Heaps
  ----------------

  There are two types of Heap:

  1. Min Heap
  2. Max Heap

  A Heap is a tree that adheres to the Heap Property, which can be defined as:

  *  Max Heap - each node is less than or equal to its parent
  *  Min Heap - each node is greater than or equal to its parent
  *  The tree is complete

If the property is satisfied than both the ExtractMax() and Insert(e) algorithms can be very quick.

The ExtractMax is easy in a Max Heap as you know that the greatest priority node will be the root node.  The actual finding of this node is therefore O(1).  The ExtractMax() method is O(log n) however, since once we have extracted the root node, we have to ensure that we replace it, otherwise the tree would be split into two seperate trees.

### Insert and Complete Trees
  -----------

Understanding how a new node is inserted into a heap explains both why the Insert method is O(log n) but also why it is beneficial to have complete trees.

To Insert an element we can just add a leaf to the first available spot.  This could well violate the heap property, so we then have to call the `SiftUp()` method to mend the heap property. This process will be O(tree height) as it will only work on whichever branch of the tree where the new element was added.

Because it is O(tree height) it is obviously beneficial to keep trees as shallow as possible, which a complete tree will always be.  In order to be complete, all leaves should be attached the leftmost possible branch.  So a complete tree would look like this:


And an incomplete tree would look like this:



There is another reason why shallow trees are of benefit, and it is that a shallow tree can easily be stored in an array.  If we look at the complete tree again and number each node with an array index, like so:




This tree could easily be represented as `[10, 9, 6, 7, 4, 6]` with the numbers on the tree corresponding to their index.  This is very useful as it makes it easy to find the parent and children of any given node in a 1-based array with these three methods:

```java
public int findParent(int index) {
  return index / 2;
}

public int findLeftChild(int index) {
  return 2 * i;
}

public int findRightChild(int index) {
  return (2 * i) + 1;
}
```

These methods would return the index of the node you wished to find.

Now that we have these methods, we can write our insert algorithm.  Inserting into the array is as easy as appending to the array, since the last space in the array will always be the leftmost free branch.  So we just need to write the `SiftUp()` method that will restore the heap property if inserting into the heap has violated it.

#### Sift Up

```java
public void siftUp(int node, int[] heap) {
  while (node > 1 && heap[findParent(node)] < heap[node]) {
    swap(heap[findParent(node), heap[node]);
    node = findParent(node);
  }
}
```

We can see clearly in this method how siftUp() works by finding the parent and travelling up that branch until the property is fixed, making it logarithmic since it ignores the rest of the nodes.

SiftUp is not just used for inserting nodes, it also is used in the ChangePriority() method. 

Changing the priority of a node can violate the heap property, so once the priority of a node is changed, SiftUp is called on that node to mend the heap.

#### Sift Down

SiftDown is another useful algorithm for sorting and building heaps.  It works on a similar principle to SiftUp, but in the opposite direction.  It is primarily used to build a heap from an array. 

```java
public void siftDown(int node, int[] heap) {
  int maxIndex = node;   // store the initial value of the node
  int leftChild = findLeftChild(node);
  if (leftChild < heap.length && heap[leftChild] > heap[maxIndex]) {  
    maxIndex = leftChild;  // reassign the variable to the node of greater value
  }
  int rightChild = findRightChild(node);
  if (rightChild < heap.length && heap[rightChild] > heap[maxIndex]) {
    maxIndex = rightChild;  // to satisfy the heap, the left child should be greater than the right
  }
  if (node != maxIndex) {  // if the intial node is not the greatest
    swap(node, maxIndex);
    siftDown(maxIndex, heap);  //  recurse until the heap property is satisfied
  }
}
```

SiftDown can be used in the BuildHeap algorithm, which would look as follows:

```java
public void buildHeap(heap[]) {
  size = heap.length;
  for (int i = size/2; i > 1; i--) {
    siftDown(heap[i]);
  }
}
```

The algorithm begins with i as size / 2 because the bottom most leaves of the heap will already be satisfying the heap property.  Because they have no children, they are automatically complete. It is the parent who is asked about their children, and so the bottom leaves do not need to be checked individually.  Because of this, BuildHeap is O(log n).

So those are the key algorithms assosciated with Priority Queues and how they can be implemented efficiently with Binary Heaps.
