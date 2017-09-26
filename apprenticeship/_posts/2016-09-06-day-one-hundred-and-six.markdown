---
layout: post
title: Day one-hundred and six
date: 2016-09-06
---

Some Thoughts on Tests
------------

We had a workshop this morning, led by Dave, on the Gilded Rose.  We talked about tests and we talked about the confidence testing gives you when refactoring a piece of code.  We also talked about how a lot of the code we will encounter as crafters will be untested.  In the afternoon some of the things we had talked about really came to the fore when I began working again on my Android Tic Tac Toe.

I've been struggling to test my app, and I think one of the key reasons is that not that much of it has been properly test driven.  A lot of my stories I've started with the very best intentions, I've written a test that I think covers the smallest part of the new functionality I want to introduce, and then I'll start trying to implement it, get to a point where I have no idea how to do something, and end up having to spike it without having the time to delete my spike and start again test driving.  This is coming back to bite me.  A stitch in time and all that.  Oh well.

As I worked on it I had the realisation that I always have when my tests aren't good enough, and that is that I love well tested code.  It just makes life so much easier, it fills me with confidence in my code, and just makes me happier to work on my codebase.

I am so used to the practice of changing something and then running my tests to check whether I've broken it, or writing something and having tests to run to check that it acts as expected, that working without that feels really hard.  I'm not sure of what I've written and I'm not sure it's going to work as I want.

That's not to say my Android Tic Tac Toe doesn't have any tests.  It does, but they just aren't the right tests.  I have more Android tests than JUnit tests, and this is annoying because the Android tests take FOREVER to run, and I want to be able to check immediatley what I'm doing.  Some of my logic is too tied up in the Android code, because I wrote it without TDD and now it's hard to test, but I couldn't quite see where to abstract, and what belonged where.  I had tried to extract as much as I could, but found it so hard when everything is event driven, and everything is so tied to the UI. I just couldn't figure out how to divide it.

I got to a point this afternoon where I could either carry on writing code and not be a hundred percent sure whether it worked, or spend however long it took to seperate my code so I could be confident in it. 

I was trying to save the state of the board, which wasn't a problem.  I was simply converting it to a String and putting into the savedInstanceState Bundle and accessing it in the `onCreate()` method.  I could then create a new Board from that String and pass the partially filled board to a new Game instance.  But then I realised that, when I create a board, I check whether it's a computer's move, and if it is, I make a move on the board.  I am doing this by accessing the `isAComputerMove` variable that is passed to the BoardActivity in the Intent.  If the activity stays alive, this `onCreate()` method is only called once, so if a computer is the first move, it makes the first move, which is displayed on the board, and then the Activity delegates gamePlay to the `onClickListener`.  But, of course, now that I was rotating my phone, this method was being called alot, and if a computer was the first player, then it would make a move every time the phone rotated.  

This doesn't make for a very entertaining game, when the computer player can have as many moves as it wants.  So I had to find a way to fix that, and whilst trying, I realised that regardless of the current player or current mark when the phone was rotated, because of how I was restoring state, when the Activity was recreated the mark would be X and the player would be whoever was the initial first player.

I was getting in a muddle, and I knew I needed tests.  I wanted tests, for my own thought process.  So I wrote tests.  Well, first I stopped trying to add new features.  I extracted every piece of logic I could into a class where it could have maybe belonged, roughly grouping behaviour, but trying not to concern myself too much with it, because all I wanted to do was to get it somewhere I could write unit tests, and be confident that all the methods I currently had were returning what I expected.  I still couldn't see where each of my methods belonged, but it felt pretty good that I could test them properly.

So, my story is taking longer than I expected, but I am hoping that now that I've got to a better place with my code, a more testable place with my code, the rest of my stories will be a breeze.  Well, in comparison to what they would have been if I had not made my code more easily testable.  It was interesting how much easier it was to see where to abstract logic when I really needed to test it.  The design is a mess, my current classes are a temporary fix, but I'm hoping that when I am happy with the code I've got so far, I will be able to see more clearly where behaviour belongs, and clean it up.  Right now tests are my focus, confidence in my code is my focus, and I'll worry about the rest when I get there.

So I guess I've just learned, again, tests are awesome.
