---
layout: post
title: Day one-hundred and forty-one
date: 2016-11-02
---

Java + Clojure Part 2
--------------------------------

I was so optimistic this morning.  I had a chat with Felipe about the state of my server and he had some really good tips on how I should think about the design, so I thought to myself, I'll just get ttt working since that's the main story this week, and then, when I've finished, I can try to change the design of my server.  I had my Clojure jar, which I could just reference in my server, right?  Right?  Right?  SO WRONG.

Clojure is hosted on the JVM.  This is a well known fact.  Clojure is compiled to .class files, so that the JVM can convert it to machine code and pass it to the CPU to execute.  A jar is a package of compiled Java or Clojure source code and its dependencies.  So, why, I asked myself mutliple times this morning, when I try to reference a method from my Clojure jar, do I get a class not found exception for Clojure?  Why does it need Clojure?  It should not need Clojure!

 This was the first challenge.  I tried to figure our what was going on, decided I has no idea how jars actually work, since they do not work the way I thought that they did, and put that on the list of things I need to look into.  Then, since nothing else was working, I added the Clojure jar to my server classpath, which didn't feel right, but was the only way I could think to get anything working again.  And then, I worked on getting one function to be executed by my server.  I wrote the function.  It looked like this:

```
(defn get-mark []
  "X")
```

Pretty complex stuff, I know.

Now that I had the Clojure jar accessible to me, I knew that I could use one style of Interop that would make my Java method look like this:

```
public String getMark() {
  IFn require = Clojure.var("clojure.core", "require");
  require.invoke(Clojure.read("tic-tac-toe.game.marks"));
  IFn getMark = Clojure.var("tic-tac-toe.game.marks", "get-mark");
  getMark.invoke();
}
```

But I don't think this looks all that nice.  I don't think I want my Server to have knowledge of Clojure, even though it now has the jar in its classpath.  I think I want to tailor my web ttt jar to my server.  I'm not sure if this is the correct way, the responsibilities are still not clear to me, but this is how I am doing it at the moment.  So, I wanted the jar to make itself as easy as possible to use.  So I did this:

```
(ns tic-tac-toe.game.marks
  (:gen-class
    :name tic-tac-toe.game.marks
    :methods [#^{:static true} [get-mark [] String]]))

(defn get-mark []
  "X")

(defn -get-mark []
  (get-mark))
```

So I'm saying that this "class" has a static method which takes no arguments and returns a String, and I wrap the function in a format that Java will recognise as something it can call.  All my tests ran and passed, so I tried to compile my jar.

Turns out that "get-mark" is not a valid name.  I guess I should've expected that, since Java doesn't like hyphons.  So I changed it, the jar compiled, I copied it over to my server and tried to import it.  No joy.  Why?  HYPHONS.  So, I went back to my Clojure and changed it to look like this:

```
(ns tic-tac-toe.game.marks
  (:gen-class
    :name ttt.game.marks
    :methods [#^{:static true} [getMark [] String]]))

(defn get-mark []
  "X")

(defn -getMark []
  (get-mark))
```

It felt really stange having Clojure and Java syntax mingling, but that's what I'm doing at the moment, because it seems to me that I'm either going to have my server knowing about Clojure or my jar knowing about Java, and I prefer the latter.  So, this compiled, I copied it over, and, mercifully, it worked.  I was returning an "X".  It felt pretty good.  I had done lots of things I did not expect to have to do and did not quite understand why I had to, but something was working and there was progress.

I then decided that it was time to get my server interacting with some useful Clojure code.  I started to write my web package, which included a function to determine the correct player.  In my cli version this is done within the recursive game loop by negating a boolean value after every turn and then assessing the value to determine whether or not it is player-one's turn.  That wasn't going to work for the web, but my Server has the board state passed to it, from which I can determine the current player.  So I created my web package, created a web-game file, wrote some tests, and got the function working.  I followed the steps I had done for the marks file, compiled my jar, and copied it over.

I tried to import the new "class" but my server was having none of it, and when I looked into the jar structure, I saw that the web package had only the .clj files, no class files.  When I compiled the jar, it did not compile the web-game.  It took me a really long time to figure out why, and I don't know if this is true for all jars or just Clojure jars, but the reason they were not compiled was because they were only referenced in my tests.  On a vague hunch (blind hope) I added a require to my core file for the web-game.  It doesn't use it, but now something other than my web_game_spec.clj file requires it, and it is compiled, and the method can be referenced from my server.

I am now able to display the current mark on the board!  Hurray!

Now that I am relatively confident I can integrate my Clojure and Java code, I can finally begin to work on actually playing the game.
