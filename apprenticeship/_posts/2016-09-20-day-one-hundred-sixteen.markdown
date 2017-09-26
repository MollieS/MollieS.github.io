---
layout: post
title: Day one-hundred and sixteen
date: 2016-09-20
---

The Server: Part One
--------------------

The beginning of my server experience has been slow.  I must admit, being given the challenge was pretty scary.  It seems huge, and complex and difficult and stressful.  Starting up fitnesse and watching all of those tests fail was a little intimidating.  So I stalled a little.  I felt underprepared, in the face of all that red, so I read up on HTTP and read through the tests and tried to figure out what the problem was. Then I tried to figure out how to start it. 

The server is a big challenge, and I'm trying to ignore this.  I want to start small and build, solve one problem at a time. There is, in the back of mind, the knowledge that as I solve problems, my codebase will get bigger, it will get more complex and, if I'm not careful, it could get messy.  Everything I write, even when trying to solve the smallest piece of the problem, has to be open for extension.  It has to be easy to test.  It has to be readable.  I've got two weeks to do this, and a holiday in the middle, so I don't want to be coming back to code I wrote over a week ago and not know what it's doing.  

In short, it has to be clean, and I want to start it well.

So, I started.  I wrote my first test, I watched it hang, I watched it fail, I watched it pass, and then I deleted it and started again.  This happened a couple of times.

I was feeling ok about the actual serving part.  As far as I can tell, there are four main steps:

1. Create a ServerSocket with a port
2. Tell that ServerSocket to listen for connections
3. Connect to the Socket
4. Do something in the connection

This was simple.  Testing was hard.  Telling the ServerSocket to listen for connections blocks the thread, as it should.  In my TimeServer I just put this in another thread, but this meant that I created a race condition, which further complicated testing.  I didn't want to do this here.  I wanted my tests as simple as possible.

I am so glad I was working on testing Android last week.  I feel like I learnt a lot whilst doing it, and I could apply it here.  I've got two main Java classes that I'm using to create a connection: ServerSocket and Socket.  But what I've realised is that I can wrap these classes, or extend them.  In some way, I can make these classes my own and make them behave as I want.  I don't have to test the intricate details of what happens when I call `accept()` on a ServerSocket, that's Java code, not Mollie code.  All I need to know is that it is called when I want it to be, and returns what I expect it to.  Every thing else I can manipulate to behave exactly as I want.  So far, my design is very much just following what Java has laid out for me.  I'm just wrapping Java objects in Mollie objects and letting them interact, but that's fine.  I'm trying to keep things simple.

So that's what I'm doing, and it feels good.  I feel that, currently, I am in control of my code, and that's a really good feeling.  Even though things are going quite slowly, I've laid some good foundations, which hopefully will minimize the risk of getting myself in a sticky mess later on.  What's more is that now, after a lot of uncertainty on how to proceed, I've got a path, and I've got a plan, so things should speed up.

I'm not quite where I wanted to be after the first full day.  I was hoping to have a fitnesse test passing and I'm not quite there yet.  I feel that I'm not too far off though, and, now that I'm feeling like I know what I'm doing a little more, I think I'm going to enjoy it.
