---
layout: post
title : Day sixty five
date: 2016-07-05 16:40:00 +1000
---

When Play Stops Being Fun
---------

I was really looking forward to using Play again.  I was determined not to hate it.  I was sure I would find its redeeming features this time around.  I would appreciate the speed of it, I would find a way for it to be easy to use.  I was sure something would click and I would be extolling the virtues of Play to everyone.

It's so horrible though.  Just setting up a Play project, for some reason, this time around, was impossible.  As I mentioned in my post on setting up a Play project, occasionally Play will highlight the index variable because it doesn't recognise it, but it usually doesn't affect the tests or compile.  It never has affected me before, and by before, I mean before today.  It took me 3 hours to run a test and I shall tell you how I achieved this miraculous feat.

During the LunchMan project, I signed up for the IntelliJ ultimate trial.  It said it was 30 days.  After one week, it told me that it had expired.  When I tried to use it afterwards I couldn't.  Yesterday afternoon, when I opened my IntelliJ CE, I got a pop up that said my IntelliJ Ultimate trial was about to expire.  I sighed and tried to open it, just for fun, and lo and behold, it loaded.

I didn't want to use it though.  It was only a day and most of the time IntelliJ CE worked fine.  Not today though, of course.  It recognised none of the play features.  It wouldn't compile, it wouldn't run any tests, it wouldn't do anything.  I tried everything I could think of, and eventually, defeated, imported it into IntelliJ Ultimate, recompliled with SBT, did an activator clean compile, recompiled my java, did a clean build with IntelliJ, and, finally, my tests ran.

So now, until tomorrow, I have IntelliJ and Play working together.  I just hope my IntelliJ CE starts behaving again tomorrow...  If it doesn't, IntelliJ will be no more useful than vim.  I will have to run my tests from the command line and will have multiple 'errors' highlighted throughout my code.

I'm also having trouble with the actual code.  I assumed that putting Tic Tac Toe on the web was going to be relatively simple.  I am familiar with the framework, I have all the logic I need, all I need to do is integrate a new input and output.

In all the web development I've done before I started from the top.  I used a framework and designed my code within that framework.  Now I've written the logic outside the framework and am trying to integrate it and I am having one of those problems that I just can't get my head around.  I am working on my human v human game.  I started by writing a test, but realised that my HumanPlayer class takes two arguments to the constructor, the mark and Input.  In the console, this input was a class that used a Scanner to read inputted text.  In my tests I used and InputFake which could set and receive data.  For the web, I have no idea what this should be.  The Human input will be the click of a button, sending a post request with the move.  How do I represent this in a class that can be passed into Human?  There is a method in the Player interface, getLocation.  In my cli version, this method called input.getUserLocation().  I have honestly no idea how this would work in my web version.  I couldn't write my test, so instead moved on to my board.

Whilst I was building my board view and working making it display marks, I realised that, for now, I do not need a HumanPlayer object.  I don't even need a Game object for the time being.  All I need is a board, the ability to make a selection, and the new board to be displayed.  I'm hoping by working on this, the Input for a Human will become clear.

Another thing that I am struggling with is that, because I haven't worked on Tic Tac Toe that much in the past few weeks, I've forgotten what many of the methods are doing.  Because I'm using a jar, I can't actually see the method implementation.  I know that I shouldn't really need to know about the implementation, but some things I do need to know.  I had forgotten that my board was immutable, and the placeMark method returned a new, updated board, and, before I remembered, I couldn't work out why the marks weren't being displayed.  At the moment I feel like I'm not only having to get more familiar with Play, I'm also having to refamiliarise myself with my own code.

I guess the conclusion of this post is that I thought this story would be relatively simple, and I think I was wrong.
