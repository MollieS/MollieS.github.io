---
layout: post
title: Day one hundred and twenty one
date: 2016-10-04
---

Server Part Four
-----------------

It was good to check in with my mentors today during my IPM.  I told them about my concerns about the design of my server, and they have given me some ideas on how to improve it.  I really want to get it to a place I am happy with it before I set out on making the last tests pass, but it's taking longer than I wanted.  I'm finding it hard to really get my head around which responsibilities lie where, but I think I've got a good idea now so hoperfully I'll be able to finish reworking it tomorrow, so that I can focus on the tests again.  As much as I am enjoying the challenge, I am looking forward to seeing all of the tests be green.

At the moment, one of my main issues is with the 418 response, which is strange, because it was one of the easiest to implement.  My issue is that, for every GET request, the process is more or less the same.  You find the resource, if it is found you return a 200 with the resource contents as the body.  If it is a partial GET then you return a 206 with the requested body.  If the resource isn't found, you send a 404 response.  But if it is a GET request to this one, specific URI, then the process is completely different.  You no longer send a 200, but a 418.  This is all working, but I feel that it could be one of the reasons my design is a little skewed, but I hope I can get my head around it tomorrow.

I also tried serving something other than the cob spec files, so decided to see whether my server could handle my blog, which it did, so that felt pretty good.  I more or less know where I want to be by the end of each day this week, which is good as I feel I have a workable time scale now, instead of just blindly hoping I can get it all done, I've just got to stick to what I've set out for myself and, hopefully, I will get everything green soon.
