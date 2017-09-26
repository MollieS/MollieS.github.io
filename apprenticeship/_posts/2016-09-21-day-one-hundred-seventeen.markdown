---
layout: post
title: Day one-hundred and seventeen
date: 2016-09-21
---

The Server: Part Two
----------------------

Today was better, faster.  I felt as if I had more direction.  One fitnesse test passed, and then another.  My initial idea was to get the server working, allowing connections, and focus on the simple responses, but as I got the 404 response working, I realised that to get any other response test working without hard coding the URI's, I had to get the file system working.

So that's what I did.  I've never actually used File IO in any language, so I spiked this to begin with.  I was wary of a "spike" that was not at all a spike, so I made a branch for my code.  Once I got the main directory served, I switched back to my main branch and started again by writing a test.

By the end of the day I had five tests passing.  They weren't the tests I had planned on getting to pass when I started this morning, but they were five tests that I am confident in and relatively happy with my code.  It needs a little refactor.  Sticking to the red, green, refactor loop has been one of my main aims of the server so far, and I think I'm doing ok with it.  It's hard when there are so many things I need to do and I am eager to get onto the next test to stop, once one test is passing, and look over my code. I'm glad I'm sticking to it though.  Most of the time, when I'm in the green, I just make a small change.  I'll just tidy up a method, or rename a variable.  The small steps make the big steps clearer, though.  Stopping also makes errors more obvious.  In my tests for serving files I was using the file path to the real public directory in the cob spec repository.  Looking over those tests I realised that, of course, I would need a relative path for these tests to run in travis or on anyone elses machine.  It was a simple error, but just something I had forgotten to think about when writing the tests.  A pause to refactor just helped me to see the mistake.

I am quite happy with the code so far though.  There is one place I think I abstracted to early, and also one class I'm not sure is adding that much value to the code.  I need to look at these.  The first is a RequestHandler which, currently, simply delegates to the ResponseBulder to build the correct response based on the resources available.  The main reason they are seperate, I think, is because of their names.  I'm not sure what the responsibilities of the RequestHandler are yet, and the fact that I am using the work "yet" means it probably should exist not right now.  I'm really trying to think of the problem one step at a time, but the future of the codebase is hard to ignore.  The second is my Resource class.  It is simply a wrapper for a File, and only really calls File methods.  I'm not sure I need this, but since all the classes that deal with Files are called "Resource" etc, it made sense for them to be returning a Resource Object.  Naming is hard and can be misleading.  I'm moving onto images now though, so I am going to leave the Resource to see if it adds any value with generalising Files and Resources.  If it doesn't, I'm going to address this.

Time is hard.  I know this is a challenge party based on the pressure it puts on you, but I'm finding it really difficult to gauge how I am doing.  I feel pushed for time, sure, but I can't get stressed about whether I'm going fast enough or not.  I'm just going to work as hard as I can to get it done and do it well, and see what happens.

I am enjoying it though.  I feel like I've learnt a whole lot, not just about Servers and HTTP and File IO, but about what I've learnt over the past few months.  I feel like I've made progress.  I think how I write code has changed.  The principles of clean code are at the forefront of my mind, most of the time.  Discipline is hard, sticking to the process is hard, but I think it's getting easier and feeling more natural, so I'm happy with that and I really hope it shows in my code.
