---
layout: post
title: Day eleven
date: 2016-04-18 18:30:40 +1000
---

IPM Monday for me, which was mainly focused on my Echo Program.  It's amazing to get such detailed feedback on a piece of code I have written.  I'm doing Rock Paper Scissors as my first story this week, and I really want to implement that feedback.  We also had a Zagaku on the Single Responsibility Principle this morning, so I would like to keep that in mind and, hopefully, not violate it in my code.

As well as that, I'm going to try and use what I've learnt from TDD by Example.  I've found that using my tests to imagine my ideal interface has been really helpful, not just for simple design, but also because I think (and hope) it leads me to better tests, which in turn should lead me to better design.  I find initial design hard, and much prefer when it evolves more organically.  This has on many occasion lead me down some tangled paths where it reaches a point where I just start again a different way.  I don't necessarily mind that, but it would be nice if I could get it right first time a little more often. 

Although my Rock Paper Scissors shares a few similarities with the Echo Program, I've started with a different idea, and it was really led by my tests.  I'm not quite sure of what my class structure is going to look like so at the moment I've got one class, the Game class.  I was thinking about what I really wanted this class to do, and I felt like its only concern should be playing the game.  It shouldn't care that this will end up being a command line application.  It shouldn't care where it is outputting do, or getting its input from.  What I want it to do, at the moment, is get two inputs, and see who wins. So that's what I'm testing.  In my Echo, I injected the console into the Echo class, but with Rock Paper Scissors I'm going to try and do it the other way around.  I'm going to have a game that, theoretically, can be played however I want, and that should be injected into the console that uses it.  This feels more natural, and makes more sense, at the moment.  It also makes testing easier.  With Echo I was not really testing what my Echo class was doing, but more the interaction with the console.  This way I can fully test the Game class and then go on to test the console.

