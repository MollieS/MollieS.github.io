---
layout: post
title: Day eighteen
date: 2016-04-27 17:28:20 +1000
---

There is a temptation, I find, when approaching a problem that I have tackled before in Ruby, to approach it as if I were still using Ruby when I am, in fact, using Java.  Ruby and Java have some similar data structures, they have some similar methods, but they are not the same.  I want to approach this version of Tic Tac Toe really using Java to my advantage, I want to learn enough about the lanhuage so that I can use it to make things easier.  I don't know if that's possible, but I'm sure that there are parts of Java that I could use and perhaps I am not yet using them because I don't know what they are yet.  I don't want to simply get a working version of a human versus human noughts and crosses game.  I want a version that uses all the tools I have properly and as well as it can.  At the moment, I feel as if I am understanding the syntax of Java, I understand the methods I am using, but I am not really delving that deep into all of the things Java can do.  I want to do more of that.

So today was the start of my Tic Tac Toe.  I began with my UML diagram, which I found hard to do since I don't think I'm very good at up front design, but I think I am more or less happy with it, and I'm not going to treat it as concrete.  It might change, it might not.  Drawing it, though, certainly helped me to begin.  Beginning is always hard.  I think everyone finds it so.  When you're faced with a blank page and just wondering the best way to go about creating whatever it is you're going to create out of nothing.  So I was glad that, having drawn my UML, I had a plan, even if that plan might change.

So I started with my Board class.  I'm unsure whether that is the best place to start.  It makes sense to me because it does not depend on anthing, but in reading about Dependency Inversion I've started to wonder whether it is best to start at the top or the bottom, as I think that starting at the top may help insure that the dependencies are inverted.  I considered this, but was not sure how I would start, so, for the sake of just starting, I began with the Board.

Now I think my Board class has all the functionality I want it to have, but it needs refactoring.  There is definite duplication in my logic for returning rows, columns and diagonals.  I've tried to make all the methods as similar as possible so that I can hopefully see a way of refactoring, but currently the solution is proving elusive.  I'm going to try again tomorrow.
