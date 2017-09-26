---
layout: post
title: Day twenty-eight
date: 2016-05-12 16:40:30
---

I've always thought that writing code is, in essence, problem solving.  You need something, so you write code to fill that need.  But what happens when you hit a problem within your problem solving endeavour?  This week I have been working on implementing a perfect player using the minimax algorithm in my Tic Tac Toe and I have been having problems.

The first problem was that I did not understand what the minimax or negamax (I've learnt they are different now) was doing as well as I though I did.  I have implemented it before, in my ruby Tic Tac Toe.  It took me a couple of days, but overall it wasn't too hard and I thought it made sense.  Coming to rewrite it in Java, I thought I would simply be able to use my Ruby code, but translate it to Java and it would all work perfectly.  So I tried that.

It most definitley did not work.

So then I had to set about trying to figure out why it didn't work.

My first issue was with the algorithm itself.  I spent some time reading about it, drawing countless messy diagrams making sure I understood how the recursion was working, and making sure I knew exactly what I wanted the code to do, and how to make it do it.  And that was ok.  I got it working.  It was passing my tests.

And then I ran the program and played my computer.  It was far from perfect.  It was choosing locations that had already been taken, choosing seemingly randomly apart from when it just repeatedly tried to choose the centre.  I looked at my code and felt that I really was not sure where these errors were coming from.  My algorithm was working, as far as I could tell, but there were too many other places that it could be going wrong.  My perfect player was depending on the GameEngine to get it's information about the board and the game was controlling the turn switching within my negamax algorithm.  Was it something to do with that?  Was I affecting the state of the GameEngine?  Why should the GameEngine be involved here at all?

So I began to limit the scope of possible issues.  I removed the dependency on game and instead passed the board to the perfect player, who also became in charge of which mark was placed on the board when.  I knew that all of the methods in my Board class were well tested and they were all behaving in the way I expected.  My GameEngine is also well tested, but since it was switching turns all the time, I was not confident in how I had affected the actual game once my algorithm had run.  Removing this possible issue let me narrow the focus on where my problem was coming from.

So now I only had two classes involved in the perfect player.  I was confident that the board was behaving as expected, and I was confident that my algorithm was working.  Whenever I passed a board into it, it would return the expected location.  All of my tests were green, but still, when I played against it in the console, it did not behave as expected.  This led me to believe there was a problem with my integration of the perfect computer and the GamePlay class, in control of playing the board through the console.

Looking at it though, I couldn't see how that would be the case.

To cut a long story short, and it did feel like a rather long story, my tests were not good enough.  All of my tests for the perfect player were testing the behaviour of the algorithm, not the behaviour of the player itself.  In each of my tests, I was creating a board with the desired state and the algorithm was returning the correct location, but when I played the player, that's where things were going wrong.

So I wrote two tests simulating the actual playing of the game, and they both failed.  I know it's not great to have more than one failing test, but I had two because I felt like to really emulate the problem I was having, I needed two.  I also knew, when I had written them, once these tests passed, I would feel completley confident that my perfect player was, indeed, a perfect player.  It had taken my almost two days to get to this point.  I had tried pretty much everything I could think of up till then, but as soon as I had those tests, I felt like I had a really strong framework within which I could identify the problem, and then solve it.

And then, of course, once I had the tests, I found the problem and fixed it in less than half an hour.  My player was persisting the best move between turns so, of course, it was always trying to return it.  The algorithm was fine, the scores hash was updating, the board was updating and the logic for finding the best move was working, but none of that would present itself when the best move was fixed on the computer's first go.  It was such a simple error, and it was solved by adding just one line of code to my perfect player class, but it had taken me so long to find.

This problem was frustrating.  There were moments that I was sure that I was, in fact, an idiot and I'd only managed to get minimax working in ruby by fluke and actually I couldn't write code at all, but once I had it working, I realised how much I had learnt from such a simple mistake.  Firstly, I understand the negamax and minimax algorithms a thousand times better than I did before, and, in doing so, I think my understanding of recursion has improved.  I am at the point now that if I were asked to write the negamax in, for example, javascript, my only concern would be that I don't really remember javascript, but the algorithm itself would be no problem (It probably would be a huge problem and take me ages to implement, but at the moment I *feel* like it wouldn't be).

I also think I learnt a lot about how I approach problems, and how I can improve on this approach, so what I'd like to remember for the future, when encountering a problem, will be a few steps.

1. Minimize the scope for where the problem is coming from.  If I have a problem and there are too many places in which it could be stemming from, there's probably an issue with my design.  If I can simplify the problem by eliminating dependencies, then that's not just good for solving the problem, but also for my code in general.

2. Ensure that any dependencies that cannot be eliminated are not the source of the issue.  I was confident, once I had removed the game depedency, that my board was working as expected because it was well tested.  Removing the switch turn depedency on the game also allowed me to be confident that I was not affecting the game state and was placing the correct mark within the algorithm.

3.  Write a test that emulates the problem.  This was the hardest part for me, because I couldn't quite see the problem.  Focussing on how to write a test for it, though, forced me to identify the cause, and therefore enabled me to solve it.  The negamax algorithm is the backbone of my perfect player, but it is not the entirety.  I was so sure that my algorithm was working that I felt as if the issue could not be coming from the perfect player class, but in writing those failing tests I was forced to look at the class as a whole, which was where the problem was.

So, in short, make the problem as small as possible, which could well end up making me find more problems, which should be split into individual problems and solved individually, and write test to emulate the problem.
