---
layout: post
title: Day seventy eight
date: 2016-07-27
---

Pokemon Manager - day one
----------

I received the challenge for my mini review board this morning.  I had known it was coming, but that didn't stop me from feeling a little nervous before my IPM.  I feel as if I still keep fluctuating between nervousness and excitement even now that I've got the challenge.  One of the points of excitement is, obviously, the content.  I am building a system for managing pokemon, which is great because I love pokemon.  A lot.

This is not the entire source of my excitement though.  I am mainly excited about having a chance to really show off what I've learnt, or try to.  The last couple of weeks I've walked into my IPM not feeling proud of what I had achieved.  I want to change that this week.  I don't expect that the code I write will be perfect.  I fully expect that my review board will point out some flaws.  I just want to be able to hand in my code and feel happy with it.  Well, maybe not perfectly happy with it, but happier with it than I have felt about my most recent code.  So I'm going to just try really hard to write the best code I can.

Of course, my code still has to work, so that's what I've been working on today.  I'm using the pokeapi API to get the information of the pokemon. For some reason trying to get my Java to send a get request and parse the JSON that was returned felt really difficult.  I wasn't sure how to go about it so I thought it would be a good idea to spike it until I had some idea of what I should do.  I had almost assumed that Java would be able to cope with JSON.  I had also assumed that getting data from the API would be simple.  I had even assumed that IntelliJ wouldn't cause a problem, since I wasn't using play.

All of these assumptions were wrong, as per usual.  I should remember to always assume the worst.  The API took me a while to sort out.  It was simple to get data back with curl and postman, but from IntelliJ I was getting a 403 forbidden error.  I thought that it was working from the command line for a time, but it turned out that it was just silently failing, which wasn't that helpful.  The fact that it was working from postman was confusing me, as it led me to assume that I didn't need any headers to get data back.  Wrong, again.  I had to set the User-Agent header to anything other than the default, and it worked.  I'm still not entirely sure why this is the case, but it is.  This is what spikes are for though, I think.

So now I was getting data back, all I had to do was parse it and get the fields that I required.  This, again, took me longer than expected.  There are lots of JSON libraries for Java but I went with the gson library, mainly because I liked the documentation and found it easy to understand.

IntelliJ did not support my decision.  I may have made an error in which version I added to the classpath, but it works with gradle so I'm not currently sure and right now I think my brain is a little too tired to figure it out.  Anyway, although it is recognising the gson library, all the classes and methods within it and not highlighting any errors, it will not run my tests.  It could be IntelliJ, it could be me, I'm sure I'll fix it tomorrow and in the meantime gradle can run my tests.

Since Pokemon is such a super cool and popular thing, there is of course a Java client for this API.  I did look at it, but I think that it just offers far more functionality than I need.  In reality, my core won't end up doing that much.  This will not be a huge app, I only have a week to build it, so it would be nice to keep it as lean as possible. I only need a find by name method, rather than the multitude of methods the client offers, so I'm going to continue to write my own.

Tomorrow will be test driving what I spiked today.  I've never stubbed an API before, but I think I have a fair idea of what I should do.  I'm hoping by the end of tomorrow I'll have a basic command line app where a user can enter a pokemon name and get their stats back, fully test driven and as clean as I can get it.
