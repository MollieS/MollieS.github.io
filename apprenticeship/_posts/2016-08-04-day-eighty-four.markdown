---
layout: post
title: Day eighty four
date: 2016-08-04
---

A Poor Man's Enum
---------------

I was pairing with Skim on my Tic Tac Toe for a lot of today, and it was really helpful.  Occasionally I've found with my ttt I know something can be improved, I understand why it should be improved, but I am unclear on how to improve it, so having someone offer a little direction was really useful.  I hope that, in time, the way to improve my code will be more visible to me, but for now a push in the right direction helps.

We focused on my PlayerFactory.  I wanted to use the Factory I created in my core in both my web and my cli versions.  The issue was that the CLI version used the HumanPlayer class, and the web version used the WebPlayer class, so how could I change these two depending on which UI was using it?

At first, we considered attempting to pass in a class as an argument to the factory so that it could instantiate the correct type of player depending on what class we passed it.  We learnt that this was not really possible in Java so moved on.

Skim suggested we try using lambdas, passing the new function into the PlayerFactory so that when it had to create a human, it would just call the function passed in which would create the correct Player type.  Christoph and Georgina told us that this was not very Java, and so we moved on again.

We decided that we would wrap the PlayerFactory in a factory dedicated to the UI it served, so that we could use the base functionality of creating Perfect and Random players, and add the functionality of the human player depending on which human we needed.

So, now that we had an idea of what out Player factory should look like, we had to decide how to differentiate between player types.  Previously I was just using ints.  This wasn't great, since I had magic numbers everywhere.  The most logical and, it seems, Java way, to check types was using an enum.  The only problem was that we cannot extend an enum.

So say we had a PlayerFactory like this:

```java
public class PlayerFactory {
  
  public static Player create(PlayerType playerType, Mark mark) {
    switch(playerType) {
      case PlayerType.RANDOM:
        return new RandomPlayer(mark);
      default:
        return new PerfectPlayer(mark);
    }
  }
}
```

And we have our PlayerType enum:

```java
public enum PlayerType {

  RANDOM,
  PERFECT

}
```

This works perfectly for our core since we don't have a human player.  If we then want to extend our PlayerFactory so that it can deal with a CLI Player, we could do the following: 

```java
public class CLIPlayerFactory {
  
  public static Player create(PlayerType playerType, Mark mark) {
    switch(playerType) {
      case PlayerType.HUMAN:
        return new CLIPlayer(mark);
      default:
        return PlayerFactory.create(playerType, mark);
    }
  }
}
```

This is fine apart from the fact that we have no PlayerType.HUMAN.  The core doesn't need a HUMAN type, since the implementation of a HumanPlayer is within the UI.  

So how can we add another field to the enum? It could be done in the core enum, but then the core would have knowledge it does not need, and since the PlayerFactory in the core doesn't have any connection to the HUMAN option, we would have to force the behaviour in order to test it.  So I went about building an extendable enum, the poor man's enum.  And it currently looks like this:

```java
public abstract class PlayerType {

  private final static String PERFECT = "PERFECT";
  private final static String RANDOM = "RANDOM";

}
```

What I like about this is that it behaves a lot like an enum.  The implementation in the PlayerFactory still works with a slight modification:

```java
public class PlayerFactory {
  
  public static Player create(String playerType, Mark mark) { //  change arg to String
    switch(playerType) {
      case PlayerType.RANDOM:
        return new RandomPlayer(mark);
      default:
        return new PerfectPlayer(mark);
    }
  }
}
```

And we can call the PlayerFactory as follows:

```java
Player player = PlayerFactory(PlayerType.PERFECT, Mark.X);
Player player = PlayerFactory(PlayerType.RANDOM, Mark.O);
```

Which looks exactly like the Mark enum, which is nice.  The difference is that now it can be extended, so in the CLI Version I could have:

```java
public class CLIPlayerType extends PlayerType {
  private final static String HUMAN = "HUMAN";
}
```

And still maintain all the functionality of the base 'enum'.

So that's where I've got to on my extendable PlayerFactory with a poor man's enum.
