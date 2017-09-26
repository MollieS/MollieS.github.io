---
layout: post
title: Day fifty six
date: 2016-06-21 16:40:00 +1000
---

Databases and Pairing
-----

Something that's come out of my IPM and retro is that I should be able to explain my design decisions and choices better.  Now that I know a little more, I know that there are different ways to do things. The more I learn, the less my code becomes about making it work, but more about making it good.  I have to think more about how I want to approach something, and why I choose one way over an other.  I have to be explain these to my mentors.  I cannot defend a choice I've made if I haven't thought about other options.

I find this easier to do when pairing, because you have two people who think differently, you will have different way to approach a problem or new feature, and we can discuss both, weigh up the pros and cons of the options, and choose one that we both agree seems best.  This is really good practice for me in thinking not only of different approaches and solutions, but also communicating them to another person.

We've started to use a database to persist our data, as we found that deploying to heroku with our CSVs caused some problems.  We had to push our CSV files to heroku to begin with so that they existed to read to and write from, but when we redeployed, we would have to push our CSVs again, thus rewriting any data that was stored there.  Heroku also starts a new dyno if one goes to sleep or we redeploy, so although the CSVs seemed a simple option when we were running it locally, they just weren't a viable option when deploying.  We had decided early on that we would use CSVs until we had a good reason not to.  Being able to deploy to LunchMan whilst it was in production was that reason.

So it was on to a database.  I was grateful to be pairing on it, since I've only really used databases with Rails before, where Active Record pretty much does everything for you.  As a result, I didn't really know how to use a database without Active Record or outside of the rails environment. 

The implementation of the database went suprisingly smoothly.  We had estimated a lot for the story, because in general, we had both found that connecting to a database, testing one and deploying with one can often be a very difficult thing.  It was difficult, but not as difficult as we had both imagined, and now we have a working database.

I feel like throughout the day, though, I let Nick drive.  I understood what we did, and if I had any questions I asked, but I'm not entirely sure I would be able to do it again on my own, so I think that'll be my weekend project, just to implement a small app with a database so that I can feel confident in using a database with Play.

I've found it very hard sometimes this week though not to rely on my pair.  I feel that he knows more about implementing a database than I do, but that doesn't mean I should try to drive.  Firstly, it's not fair on him.  Pairing is exhausting, you don't get the mental breaks that you do when working alone.  Forcing my pair to drive by not offering to do it myself forces him to be completely engaged at all times.  It's also not fair to me.  I wonder if I had offered to drive more, work with a navigator's advice, I would feel more confident with databases.  Watching someone do something, not matter how envolved you are in the process, is a poor substitute for doing it yourself.

So, in conclusion, I feel that I have not been a good pair this week.  I have learnt a huge amount, but maybe not as much as I could have, and I have forced my pair to be more active than myself.  I hope I can rectify that in the next two days.
