---
layout: post
title: Day one-hundred twenty nine
date: 2016-10-17
---

Playing with Recursion
--------

In my brief time learning Clojure, I have decided that recursion is the answer to everything.  Programming without being able to redefine variables and without state feels very new and different to me.  Fortunatley, recursion is not excactly new to me.  I can't say I am a master of recursion, because sometimes I still find myself staring at a piece of code and struggling to work out what is happening where, but I think I'm getting better.  When I was reading about Big O Notation, I spent a lot of time looking at algorithms and trying to figure out how they worked, and I think this has helped me visualise what is going on when I use recursion.  Or maybe I just understand it better now than I did before.  Regardless, using recursion is getting easier and that very beneficial when it comes to Clojure.  

I've come across two problems so far that I have had to solve differently, and both are to do with maintaning state, and both were solved with recursion.

## Redefining Variables
---------------

It's pretty easy to define a variable in Clojure but because everything is immutable, you cannot reassign it.  This became a problem in Roman Numerals.  In Java I could write something like this for numbers up to 3:

```java
public void convert (int arabic) {
  String roman = "";
    for (int i = 0; i <= arabic; i++) {
      roman += "I";
    }
  return roman;
}
```

In Clojure, this isn't possible for many reasons, but I began to try and do something similar.  The issue was I could not reassign the roman variable or add to the roman variable.  If I declared roman to be an empty string, it would always be an empty string.  But recursion came to the rescue:

```
(defn convert
 ([arabic]
  (convert arabic ""))
 ([arabic roman]
  (if (= arabic 0)
   roman
   (recur (dec arabic) (str roman "I")))))
```

Here the value of roman is changing, depending on what I pass in.  It is, in a sense, reassigned, but only as it is passed as an argument.


## Maintaining State
-----------------

I knew in Tic Tac Toe the persistance of the board was going to be a problem.  In Java, adding a mark to the board was easy.  I could do something like this:

```java
private List<String> board = new ArrayList();

public void placeMark (int location, Mark mark) {
  board.add(mark, location);
}
```

In Clojure, again, once board was assigned, it would stay assigned.  So once again, recursion came to the rescue and I am currently doing something like this:

```
(defn place-mark
  [location mark board]
  (let [marked-board (assoc board location mark)]
  (recur (reader/get-location) (game/player-mark) marked-board)))
```

So, instead of having some variable that I add to, I have the argument that is passed in.  This is fine, but it means that placing marks on the board is inherently tied to the input-reading function and printing the board.  In my Java Tic Tac Toe my cli app used elements of the game but I tried to keep them as seperate as possible.  With Clojure I am finding it harder to keep them seperate, but perhaps that's ok.

