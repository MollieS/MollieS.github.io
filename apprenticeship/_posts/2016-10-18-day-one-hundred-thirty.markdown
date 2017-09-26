---
layout: post
title: Day one-hundred and thiry
date: 2016-10-18
---

Clojure TTT
-----------

Today was all about Tic Tac Toe.  I had my command line app working, in that you could place marks on a board and see it, but I hadn't implemented any of the rules.  I worked on that this morning and, having said yesterday that recursion was the answer to everything, didn't use any recursion.  Something I like a lot about Clojure is that it has a lot of core methods ready for you to use.  Coming from Ruby and then into Java was a bit weird for me because Ruby has methods for everything.  It felt like anything I wanted to do in Ruby was already built in.  It's quite similar in Clojure.  I know that Java had a reduce function, but I didn't really use it.  In Clojure I feel like I'm using it all the time.  It's so useful!  I feel that my Clojure Tic Tac Toe is so much simpler than the Java version.  In Java I had for loops everywhere.  I did, eventually, change these to streams, but it still feels like Clojure makes it easier.  Plus, getting the columns from an array was so easy I almost didn't believe it would work, but it does!

```
user=> (apply map vector (partition 3 [1 2 3 4 5 6 7 8 9]))
([1 4 7] [2 5 8] [3 6 9])
```

What I'm finding hard about Clojure is making it readable.  So far I've found it relatively straightforward to read code and examples.  I think I am getting my head around the syntax, but I don't think my code is that readable.  Like the example above.  I know it works, but reading it I wouldn't really be sure what it is doing.  Maybe that's because I still don't know Clojure well enough.  I'm trying to use small functions with meaningful names to make it clear what's going on, but I feel it's not intuitive.  I guess that's just one of the differences between languages like Java and Clojure.

Another thing I am currently unsure about is how many functions I should be chaining together.  This ties in with making my code readable.  Sometimes I feel that I've got too much going on in a line.  There'll be a map with a custom function and then I'll reduce the result and it ends up being quite hard to follow.  I'm trying to seperate these out, but that makes my functions quite long and I'm not sure if that's the Clojure-like way.

One of the things I'm struggling with is that I want to do things right, I want to do things in the proper way, and I feel like I don't know enough about the Clojure way yet.  I'm working through Clojure for the Brave and True but there's only so many hours in the day and I feel like I can't get through it fast enough.  When I was learning Android I felt that the more that I read from the Android Programming book, the more I realised the mistakes I'd made in my Tic Tac Toe. The more I learnt the more I could see better ways to do things I'd already done.  I know this will be true for pretty much everything I do in life, but I suppose it's about finding balance.  Knowing enough to get something working isn't the same as knowing enough to do something right and I suppose that, whilst I'm learning, I should give myself time to be able to go back when I learn a better way.  I know that once I have written a piece of code I can always go back and change it, but it's just giving myself time to do it.  It's sort of part of refactoring, but I don't think it is really refactoring.  I suppose it's more about maintanence and improvement.  But then again, in Clojure it feels easier because there is less code.

