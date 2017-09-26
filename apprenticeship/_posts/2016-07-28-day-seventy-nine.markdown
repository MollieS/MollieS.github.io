---
layout: post
title: Day seventy-nine
date: 2016-07-28
---

Pokemon Manager - day 2
------------------

Today was one of those days where I found it hard to get a really good stretch of work, and yet seemed to still get a lot done.  I'm not quite at where I wanted to be by the end of the day, but I am relatively happy with what I've done, and all the functionality for my first story is there.

You can now search from the command line for a pokemon and see a few details about that pokemon as a response.  The command line application hits the API for its information, but my tests do not.  I was a little unsure whether I had stubbed the API behaviour in the correct manner, but I think I feel good about it at the moment.  I'm happy with it for two main reasons.

Firstly, my fake implementation of the class that calls the API is completley interchangable with the real class.  I know that this is the point, but the ease with which I can change them feels good.  

Secondly, both my fake and real implementation return Json for another class to format and filter.  I was initially a little worried about this.  I felt that, since they were dealing with Json in the first place, perhaps I should contain the dependency on the gson library to one class, and instead of returning Json, just return the strings I wanted.  I decided against this because I wanted to make sure that it was as easy as possible to test the methods that found the correct values from the Json.  The PokeApi returns quite a large Json object, and it felt quite complex to get the values I wanted.  If I had kept all the methods for retrieving the Json and finding the values from the Json, I think I would have had a lot of duplication between my real and fake implementations and, more importantly, the class would have had too many possibilities for change.

So, unless I have a made a mistake somewhere I haven't thought of, which is entirely possible, I'm going to leave my core alone.  I felt like a piece of understanding really slotted in whilst doing it, so I'm hoping if I have got it wrong, I haven't got it too wrong, since it would mean my understanding was way off.

I still feel a little nervous that I'm not testing the actual API call, but I think that if I have an integration test in my CLI app then that will be sufficient.  If the API changes in someway, my tests will still pass, but I think that is just part of relying on third party services, and also has helped me to understand the importance of versioning.

So at the moment, I'm feeling relatively happy with my Pokemon Manager.  There's still work to do and I'm hoping it won't take me too long to get the CLI interface working. I'm trying to keep it as simple as possible and make sure that everything is test driven.  I feel that it will be similar to my Tic Tac Toe CLI, but at the moment, and maybe just because it's smaller, I'm confident that my core does not have to know anything about the type of display I'll implement, which was a problem for my Tic Tac Toe web version.

Tomorrow I will get the CLI version to where I want it to be and add tests and functionality for searching for non-existent pokemon and then I will start working on adding and persisting caught pokemon. I'm still not sure exactly how I want to persist the data, whether in a file or a database.  I know that a file would be simpler, but I also want to learn more about using databases, so I will address that decision when I come to it.
