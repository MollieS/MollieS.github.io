---
layout: post
title: Day seventeen
date: 26-04-2016
---

A little more Rock Paper Scissors refactoring this morning, which was really useful.  In the back of my mind I'm thinking about my Tic Tac Toe UML and I think the refactorings I'm making to my Rock Paper Scissors will crossover. I think I've more or less finished with Rock Paper Scissors now, but again I'm finding it so hard to say I am done with it, so I'm going to move away from it tomorrow, onto my Tic Tac Toe, with the knowledge that if I really want to, I can go back to it.

In the afternoon I started researching for a Zagaku which I want to do next week, so I won't write too much of what I've learnt here, but I was reading about the Dependency Inversion Principle.  I had previously learnt about Dependency Inversion when I was learning Ruby, but it was hard to really grasp it when Ruby doesn't really support abstraction in the same way Java does.  I've used Interfaces now, and can see more easily what they do and why they would be used.  I haven't yet used an abstract class but I think I understand the difference, after a bit more reading, so I can see how and why I would use it.

As I understand it, an inteface is like a template for any class that implements it.  It is a contract that promises that any class that uses that interface will have a certain set of methods or attributes.  It defines what those methods will return, if anything, and what arguments that method will take, but the content of the method can differ from each class that implements the interface.  This is useful for related classes that share some behaviour, and means that each of those classes can be used by another class with confidence about the methods that it will use.

And abstract class is useful for more closely linked class types.  In an abstract class you can define a method and the content that each class that extends it will use, and all classes that extend the abstract class will default to the defined method.  This means that when a method defined in the abstract class, the behaviour will be the same regardless of who is implementing the method.  

There are lots more technical differences between them, but to me these seem like the key differences of when I would use one over the other.

So onto Tic Tac Toe tomorrow...
