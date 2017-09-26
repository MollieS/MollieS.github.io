---
layout: post
title: Day twenty one
date: 2016-05-03 13:40:40 +1000
---

Naming is hard, we all know that.  I don't think I am particulaly good at naming, and that's something I need to work on.  Naming, I think, is important not only for the readability of your code, but also to aid your design.  If you anme something wrong, or give something a name that is misleading or too vague, then the code within could well take on that characteristic.  If you are not sure about a class or method should be called, then it seems that you are not clear on what that should be doing.

In my Tic Tac Toe I have a interface that, currently, has the simple responsibility of outputting strings.  So it's named Ouputter (output was taken.  I am not great at naming).  I named the interface, and now it outputs strings.  This is not what this interface should be doing.  It should be interacting with the user and displaying information.  In limiting it so heavily I have forced another class to have responsibilities that it should not have, violating SRP.  In making changes to my code, the name of the interface was confusing me.  This interface is called Ouputter.  The classes that implement it are simply outputting strings.  How can I make it do more?

So, I renamed it. The interface is now called Display.  The classes that implement it now implement a display.  I can start to introduce more functionality to the classes that implement display, because their behaviour is no longer limited to simply output. Hopefully this will allow me to see more clearly which behaviour can be moved because the name is more descriptive of what I want it to be doing.
