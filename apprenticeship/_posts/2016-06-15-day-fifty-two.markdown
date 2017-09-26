---
layout: post
title: Day fifty-two
date: 2016-06-15 18:40:00 +1000
---

# Stacks
---------

The defining feature of a stack data structure is LIFO, last in, first out.  A stack acts exactly as the name suggests.  Given a stack of paper, the last piece that you put onto of the stack will be the first piece of paper that you pick up.

A stack has four main algorithms associated with it:

1. Push: which adds an element to the stack
2. Top/Peek: see which element is at the top of the stack
3. Pop: take the topmost element off the stack
4. Empty: returns whether the stack is empty or not

The Stack is considered a linear binary structure, and may be implemented with a set capacity, so that attempting to add to the data structure when it is full, would cause an overflow state.

Many languages accomodate the stack data structure and the essential algorithms that are used with it.

# Usage
----------

In the data structures and algorithms course, they came up with a really good example of when a stack is useful, so I want to relay that here.  I have a real tendency to use the data structures that I am familiar with, usually arrays and hashes, but it's important to remember that there are many other data structures that exist, and could possibly do the job better.  So, by recording this really good usage of the stack, hopefully if I come across a problem that requires a similar solution, I'll remember the stack.

What's especially good about a stack is that the four key operations above are all O(1).  Because there's no finding in or iterating over a stack, every operation is very fast.  The stack data structure keeps a pointer to the top element, so when we push to the stack, we already know where that element will go.  If we peek at the stack, again we know which element we need to look at.  The same goes for popping off the stack, and for checking whether the stack is empty.

### Unmatched Brackets
----------

Consider we're writing an IDE or text editor, and we want to flag when a bracket is unmatched.  A stack is the perfect data structure to use to check our brackets.  Let's say we want to implement a method that checks all the brackets in a text.  If there is an unmatched bracket, then we want to return the position in the text where the unmatched bracket is, and if all the brackets are matched we simply want to return the string "Success".  We can use a stack easily to implement this:

```java
public String checkBrackets(String text) {
  Stack<Bracket> openingBracketStack = new Stack<Bracket>();
  for (int position = 0; position < text.length(); ++position) {
    char next = text.charAt(position);

    if (next == '(' || next == '[' || next == '{') {
      openingBracketStack.push(new Bracket(next, position));
    }

    if (next == ')' || next == ']' || next == '}') {
      if (openingBracketStack.isEmpty()) return String.valueOf((position + 1));
      Bracket top = openingBracketStack.pop();
      if (!top.Match(next)) return String.valueOf((position +1));
    }
  }
  if (!openingBrackStack.isEmpty()) return String.valueOf((openingBracketStack.pop().position +1));
  return "Success";
}
```

# The Stack
------

I wanted to make sure that I really understood how a stack data structure worked before delving into how the memory structure works, but I don't think I know enough about the stack as a memory allocation device yet to write about it.  But hopefully I've demonstrated the key features of a stack and when one can be used, so that when I do know enough about the stack to write about it, I will have this to refer to.
