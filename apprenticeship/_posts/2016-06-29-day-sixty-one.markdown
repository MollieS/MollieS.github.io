---
layout: post
title: Day sixty-one
date: 2016-06-29 16:40:00 +1000
---

Liskov Substitution Principle
--------

At the heart of the Liskov Substitution Principle is the idea that subtypes must be substitutable for their base types.  When the creation of a derived class forces change in the base class, then we know that there is something wrong with our design and implies a violation of the Open Closed Principle.  Satisfying the LSP allows all functions that reference base classes to use objects of derived classes without knowing it.  Regardless of the type, the client knows that the behaviour will be the same.

The difficulty with LSP is that we cannot judge the validity of a model in isolation.  Issues only really become visible in the relationship with the client.

### An Example
----------

We have a dragon.  It breathes fire and it flies, as dragons do.

```java
public class Dragon {

  private int fireIntensity;

  public Dragon(int fireIntensity) {
    this.fireIntensity = fireIntensity;
  }
  
  public int breathFire() {
    return fireIntensity;
  }

  public void fly() {
    //...
  }
}
```

We also have a town.  The town often suffers attacks from the Dragon.

```java
public class Town {

  private int damage;

  public Town() {
    this.damage = 0;
  }

  public void attack(Dragon dragon) {
    damage += dragon.breathFire();
  }
}
```

After a while we decide we need a new dragon.  A Nightfury is a specific kind of dragon, but it is still a dragon.  It still breathes fire and flies, but is more powerful than your standard dragon.

```java
public class NightFury extends Dragon {
  public NightFury() {
    super(20);
  }
}
```

The Nightfury is very effective at destroying towns.  It is very powerful and also never misses.  But now the townspeople need a dragon to help them, otherwise there will be no towns left.  Gyarados is the answer.  Gyarados will fight for the townspeople and we all know that dragons are supereffective against other dragons.  One of the great things about Gyarados is that they don't breath fire, they are water dragons, so can help the townspeople by drowing the flames of other dragons.

```java
public class Gyarados extends Dragon {
  public Gyarados() {
    super(-10);
  }
}
```

This is a violation of the LSP, but it's very difficult to tell just from looking at Dragon.  To really understand why this violates LSP we have to look at what is using Dragons, and design by contract.

Design by contract is an idea expounded by Betrand Meyer stating that methods of classes declare preconditions and postconditions.  A precondition must be true for the method to execute, and the method guarentees that the postcondition will be true.  Meyer says:

*When redefining a routine [in a derivative] you may only replace its precondition by a weaker one, and its postcondition by a stronger one*

Looking back to town, we could say that the postcondidion of the attack method is that the damage to the town increases.  When a town is attacked by a dragon, it is damaged.  But Gyarados exhibits different behaviour, and this is the key.  Although Gyarados is a dragon, it behaves differently to how the other dragons behave.  THe LSP demands that users of the base class must not be confused by the output of the derived class, and that is exactly Gyarados' behaviour is confusing.  So although Gyarados is a dragon, it is a different type of dragon, and must be treated as such to adhere to the LSP.
