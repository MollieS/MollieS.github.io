---
layout: post
title: Day sixteen
date: 2016-04-25 +18:27:18
---

Most of my IPM was focussed on feedback on my Rock Paper Scissors.  It is so good to receive feedback when I can really understand why it is a good idea to change something.  I feel like I have got feedback in the past where I've thought "this person knows a lot more than I do, and they are telling me I should change something, so I will change it." without fully understanding the whys.

Yesterday was a different story.  Everytime something that could be improved upon was pointed out, I could see it.  I could see the reasons, and, mostly the solution.  It felt great. I hope this is a sign that I'm understanding more.

So there are 3 main things on my RPS refactor list.

The first is to use a factory pattern to remove the dependency between my game and my rock, paper and scissors elements.  Currently, the Game creates these objects and so must know about all of them.  It doesn't need to do this.  All it needs is two elements.  It doesn't care what they are.  I've never actually implemented a facotry pattern before, but this seems a perfect time to learn, since I can see the need for it.  Well, it is not necessarily needed, Rock Paper Scissors is small enough to work without these Element objects, but still, I can see why a factory is better than the current set up.

The second is to implement a Human Player as well as the Computer Player.  I started with no players at all, and it was only when I was stubbing the random behaviour of the Game that I extracted the random throw method to a class, which left me with a Computer Player, but no other players.  Obviously, this seems a bit strange, so I'm going to introuduce a Human player and remove the randomization and stubbing from this level, injecting a randomizer to the Computer Player instead.

The third main change I am going to make is to refactor the Console.  At the moment, my App class has quite a few concerns, and the Console can take care of a few of these.  Also, the way I am testing the console is by passing a Test Console into my App class.  It would be better to inject different input and output streams to the console.  This seems like the biggest task, so I'll divide it into two:  I want to reduce the concerns of App using Console, and I want to change my Console structure.

I hope I have time to move onto the refactoring of the Console, but I can't spend too much time refactoring of RPS as my Tic Tac Toe in Java will be started this week, and I've planned to get a Human v Human game working by the end of this iteration.
