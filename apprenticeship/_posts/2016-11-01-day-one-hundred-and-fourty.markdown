---
layout: post
title: Day one-hundred and forty
date: 2016-11-01
---

Clojure + Java
-----------------------

It is time for my Clojure tic tac toe to enter the browser.  I am attempting to serve it with my HTTP Server and, so far, I've spent a lot of time umming and ahing.  It's hard to know where to start.

After a while, I decided to start with the server and add a new route, and the html to go along with that route.  I added a menu which passed params defining which game type the user wanted to play to the game route.  The game route has some buttons that, when pressed, send params with the selected button and the current board state.  Of course, there's not much going on since I haven't hooked my game logic to any of this.  Once I had my server serving static html files, I realised that for the game I was going to need to change what was displayed in the browser.  I don't know how, or it is possible, to interpolate Java with a static HTML file, so I changed my game route to dynamically generate the HTML.

So now my server can update the view with an X when a button is pressed, and the route is not persisting any state.  This, I think, is good.  I'm happy with it anyway.

I've been working mainly with my server as I am currently pretty uncertain of how to connect my Clojure jar my server.  I have seperated my cli code from my game logic in my Clojure project, and for a while today I was considering beginning to write my web human player, but I got confused.  Very confused.  

What I would like for my human web player to do when making a move is to take the String that is passed as a parameter, parse it and return the integer.

Currently my Clojure tic tac toe has no real concept of players.  The cli game runner simply receives an array of functions.  If it was player one's turn, it would call the first function in the array.  If it was not player one's turn, it would call the second function.  Both functions take the same arguments, the board and the current order of the marks, and returns a number.

The problem with my web player is that it cannot act in the same way as the computer player.  It does not need to recieve the board as an argument, and it doesn't need to view the marks.  All it needs is the param passed in.  If I wanted to pass a set of functions in the web version, then I would have to give it two arguments it didn't use and add an argument to the computer and cli players that they would not use.  That's not right.

Another thing I'm currently struggling to get my mind around is where the responsibilites of the game lie.  How much should my Clojure jar know about the web?  

This is complicated by a design flaw in my server.  What I would like to have is a server and then Controllers on top of that.  In my server, however, the controller functionality and the server functionality are intertwined in the routes.  This makes it tricky.  Some parts of my tic tac toe need to be in the server, but it feels strange for the server to know about them when really it should be the Controller that is responsible for them.  This is making me lean towards wanting to put the functionality in my Clojure jar, but maybe that's not where it belongs.

The distinction between the web and my jar is also made very clear by the fact that they are in different languages.  I'm not really thinking of how to connect the two at the moment, just trying to make sure I have all the functionality I need in the right places for when I do connect them.  When I was working on my Java web version, the distinction between the web and the jar was not quite so stark.  I put my web player in the web respository, because that seemed like where it belonged, but now I feel like I'm having to think harder.  I feel like I should have my core logic, a cli jar, and a web jar.  But then again, perhaps it would be better to encapsulate all the web logic in the code that deals with the browser?  I am still not sure.  

I suppose I don't have to make a decision right now.  I will try and work out how to get my server and my jar talking to each other, and then I'll see how I feel about where this pesky web player should go.
