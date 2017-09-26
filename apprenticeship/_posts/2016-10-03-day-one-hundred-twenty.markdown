---
layout: post
title: Day one hundred and twenty
date: 2016-10-03
---

Server Part Three
------

I came back from my holiday and was straight back into the server.  It was good to have a break from it, although some of my momentum may have been lost.  I had one test that I wanted to have passing by the time I left, but I didn't.  It didn't take me that long this morning to figure out the problem and get it passing, but then I started thinking more about my code.  I guess that's one of the good things about getting some distance from it.  I've been so caught up in the details and the tests and thinking how to do something, that the bigger picture was pushed to the back of my mind.

Some parts of my code, I like.  I like that adding a new route with new functionality is easy.  The only change I need to make in my code is to the main function to tell it that I am registering a new route, and then I can create the class without affecting any more of my existing code.  This feels good, and I can be confident that adding a new feature will not break any of the existing functionality.  The issues I'm having are these:

1.  I think my routes are doing too much.  When I was just dealing with serving files, it made sense that the finding and reading of the files were connected to the HTTP responses I was sending back.  When I got to the form route, though, it felt like this was doing too much.  I was performing an action, and also returning the response.  In many ways this makes sense, but in some ways I worry that I am violating SRP.  What I want to do with the data could change.  I could need to check the data, rather than just putting it in the body, which would change the code, but the responses would stay the same.  There's more than one reason I would have to change the code, and I don't like it.
2.  I think this is connected to the first issue: I've got some duplication.  The logic for sending a response for a static file and a custom route are the same, but each is implementing them seperately.  I would like to generalise this, but I can't quite see how or where the abstraction should be.
3.  I sometimes feel that I'm not implementing something quite right.  I've got all this information on HTTP that I am trying to get through, but some of it is just really confusing.  When I was working on serving the file contents, I had to implement the Content-Type header.  This works for all the files, but it doesn't work for some other routes, and it should.  I can pass the tests, but I also want my server to work properly.  The cob spec tests don't cover every aspect of the responses for every route, but that doesn't mean they shouldn't work perfectly. If I'm building a server, I don't want it to work most of the time.  I want it to be right all of the time.  It is sometimes hard to remember everything for every route though, so I'm hoping I'll have some time to go over each element and make sure it works perfectly, not just enough to pass the tests.

In general, I feel that there are issues with my code.  Something is just not sitting right, but I can't quite see how to fix it.  I don't want to rush in too soon and pull code out to somewhere it shouldn't be.  I'm hoping that if I carry on, I will see where I should make a change more easily.  I've still got seven tests to go, and they are mainly the ones I've been dreading as I have no idea how to implement them.  But so far, I haven't really had any idea how to make any of them pass, and they're passing, so I'm hoping it'll just work itself out.  I've got all the information I need, and I think I am capable of figuring it out, I've just got to do it.  

I am really enjoying the challenge, it feels big, and I feel like I've got an infinite amount of code at the moment, but it is fun.
