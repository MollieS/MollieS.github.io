---
layout: post
title: Day eighty two
date: 2016-08-02 18:30:00 +1000
---

Pokemon Manager - part 4
-------------------------------

I think about me considering a web interface yesterday and laugh a little bit.  There is no way I would have had time.  I finished the freeing pokemon this morning, and reviewed some of the comments on the pull requests.  Felipe suggested I look into someway of reducing the duplication in my DBManager, where many methods have the same structure.  They open a connection to the database, create a statement, do something, close the connection and close the statement.  I talked to Rabea about it and she suggested that since Java 8 now has lambdas, I could pass the do something part into a base method that deals with opening and closing connections.

I had thought about this previously, since I know I've done something similar in another language.  When it came to Java though, I just got really confused.  I couldn't work out how to do it, so, to save time, I abandoned it and asked Georgina to pair with me using Java lambdas next week so hopefully I'll understand soon.  I think they are quite similar to Ruby lambdas, there just seems to be an added level of complexity that I couldn't figure out.

Whilst I had been working on the freeing pokemon story, I had noticed that my PokemonManagerCLI wasn't really how I wanted it to be.  When I started, the only functionality was searching, so it made sense to have only one class.  As the application grew, I was just adding to that single class and once I had got the freeing pokemon working, my ApplicationRunner was in charge of far too much.

Within the ApplicationRunner I had started to seperate the concerns into different methods, but I felt like I needed to abstract them further, so I did.  I ended up making a Page interface.  The ApplicationRunner would return the correct page depending on what action the User chose, with a Navigator class, that was pretty much just a Page factory, dealing with the creation of pages.

I don't know why, but I really enjoyed reworking the code.  As I seperated the pages, each of them became so much easier to test.  I was also just really glad that I didn't hand in the giant ApplicationRunner class, it was violating SRP and OCP, but I noticed it!  That felt really good.  And as I reworked the code, I noticed a few things.  The Page interface and seperation of different pages would make it a lot easier to add new functionality, hopefully satisfying OCP, and the seperation was helping to restore SRP.  It's not perfect, and I actually ended up feeling a bit short of time, but it is better.

It just felt good that I had noticed the error, and I think I learnt a lot more about the SOLID principles by feeling the need for them.

So Pokemon Manager is finished.  I think I did the best that I could, so I am happy about that.  I feel like I could've spent more time going through the code, but I think that's just how you feel sometimes.  It doesn't feel finished, but I know even if I'd had 3 weeks it probably wouldn't have felt finished.

And now I have a fully functioning pokedex on my laptop.  Amazing.
