---
layout: post
title: Day one-hundred and thirty-six
date: 2016-10-26
---

Back to Java
--------

The past three days I've spent away from the office, helping train developers to write better Java.  We talked about Clean Code, TDD and functional Java.  It was a busy three days, so I didn't find time to write a full blog post daily, so here's a summary of what we did:

## Day One:
-----

This was the day we talked about the SOLID principles.  The group we were training had a pretty varied mix of abilities.  Most were not familiar with Java syntax, although some had come from C# so it wasn't too dissimilar.  Very few had heard of the SOLID principles, so I think trying to cover them in such a small space of time was hard, but over the course of the three days I think we gave them examples that not only adhered to the principles we talked about, but also violated them.  The experiences they had with the different types of code, I think, really showed them the benefits of working with clean code.

Danny had written a chat server that, in the afternoon, we mobbed on.  We were meant to have 12 attendees but one was ill, so we divided into groups of 4 and I joined the group of 3.  This was really fun, and I really enjoyed being able to guide the people I worked with to see the principles we had been talking about in the code.  The aim of this exercise was to add a new feature or two to the chat server.  This was relatively simple since the code was good, well tested and the responsibilities were clear for each class.  Since we were adding new features that were not too dissimilar to the existing features, the code gave clues as to where the new functionality should go.

I felt the others in my mob group had a tendency to look to me on advice on how to proceed, and it was a challenge sometimes to push them to figure it out for themselves.  I was more than happy to help them with Java syntax when they got stuck, but with design decisions I didn't want them to lean on me, as I felt they would learn more figuring it out for themselves.  This was a tricky balance, advising, helping and teaching, but letting them make mistakes and learn from them.

## Day Two:
-----

Danny and I live paired on our Tea Shop example in the morning.  Sometimes I get nervous writing code infront of others, I certainly did performing my kata on Friday, but I didn't feel at all nervous working on the TeaShop.  I was familiar with the domain and trusting my Java knowledge.  I suppose I felt a little like I had to prove my worth to the attendees of the training session since I am an apprentice, so I didn't want to make a huge mistake, but it was fine and went well.  They watched us practice strict TDD and participated in any design decisions we made.  Danny and I haven't really paired since I've been at 8th Light, but we both trusted each other to be disciplined in our approach to writing code.  I think this is one of the great things about the apprenticeship.  Everyone we work with has been through it, so we know that they approach writing code in the same way, with the same discipline and the same goals.  We deliberated design decisions and, when it was not clear where we should head next, we discussed it, but I hope it was beneficial for the attendees to see how simple it can be to pair with someone when you both have the same goals in mind.

The afternoon was another mob session on the video store refactor.  This was harder and I think all the groups struggled.  We'd gone through refactoring patterns, and now we were trying to implement them.  It was interesting how all the groups found it harder, and I think it was a great comparison with the chat code from the day before, as they could see how much easier it is to work with clean code.  I joined a mob group again, and found they were relying on me quite heavily.  At each turn we had long discussions on what to do next, and it was hard to encourage them to discuss ideas without my input.  Progress was very slow, but I think it was useful for them.  When they were discussing ideas I would try to prompt them to think back to the SOLID principles, to look for code that violated a principle, to think about why the code was difficult to work with and how it could be easier.  This was good learning for me too, and challenging at times.  Mobbing is really fun, but in a group with such different abilities, on a code base they are not familiar with, and a new language, it was also really hard.

## Day Three:
--------

I presented on functional Java in the morning, using examples from the Tea Shop domain that we had begun the day before.  It was difficult to really emphasises the changes between the imperative and functional styles of Java, as many of the attendees were so unfamiliar with Java syntax, so I really tried to focus on the theoretical differences.  For example, streams in Java behave very differently from loops, and so that was what I was trying to focus on.  The behaviour underneath and the implications of that in how you program.  I'm not a huge fan of public speaking and I was nervous to begin with, but actually, by the end of my presentation I was quite enjoying myself.  I found it quite hard to tailor the content of the talk for them.  It would be very different to give the same information to a group of people who were familiar with Java, but my audience were not, nor were they familiar with Functional programming so, although they might not remember the syntax, I hope the ideological differences became clearer.

We then started again on the chat client.  Everyone had really enjoyed working on it on the first day, and they were familiar with the domain, so it made sense for them to try to implement a bigger feature with what they had learnt.  They mobbed and Danny and I paired, our code on the screen, so that, if they wanted, they could watch how we were approaching it.  It was a really fun project.  We were adding a hangman bot to the chat server, and I think everyone was excited about the feature.  At the end, we talked about our approaches.  Some of the groups had got it working, and the ones who hadn't were close.  I think it was really beneficial to present them with the solution that Danny and I had come up with.  We had all been working on the same problem, so we were all familiar with it, and they could see the differences between their approach and ours, and understand how our approach had made things easier for us, and how we had written our code so that further extension to the chat client was still easy.
 
------------------------

I wasn't really expecting to enjoy the training so much.  To begin with I was worried that the people we were training would be disengaged, but actually they were all very attentive and eager to learn, which I think is part of the reason it was so enjoyable.

It was also a great learning oppertunity for me.  Helping other people is a great way to cement knowledge, and it was really nice just to be able to help.  I think it was a confidence boost for me just to realise I do know some stuff about Java.  I mean, I knew I had some knowledge, having been doing it for over five months, but it was good to feel that I had enough to help others learn.

It was also my first taste of real client work.  I know that most of the client work we do is very different and training is a specific kind, but it was my first time going out and representing 8th Light and I hope I did a good job, plus it was fun so I hope I get to do it again
