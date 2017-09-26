---
layout: post
title: Day thirty seven
date: 2016-05-25 17:15:00 +1000
---

Today was a hard day.  My brain would not quite do what I wanted it to.  It took me far longer to do anything than it usually would.  It was frustrating and exhausting.  I do not like being ill.

Regardless, today I was working on making my board immutable and I couldn't help but wonder why, which to me, marks a pretty big step in my journey in learning to code.

When I first started coding, I didn't really ask why.  I absorbed as much knowledge as I could.  I trusted my coaches.  I accepted that what I was told was the best way to do things, because I knew nothing.  I didn't know enough to ask why.  Now I do, and I feel confident enough in my knowledge to question the new things I have learnt and the things I learnt before.

Of course, I have retrospectively questioned most of the things I've learnt so far.  I wouldn't have ended up at 8th Light if I hadn't really thought about what I knew, what I wanted to learn, and what I loved about writing code.  I was lucky that the things I had accepted blindly to begin with turned out to be so important to me and my code.

But today it wasn't that I was asking if my mentors were wrong in suggesting I made my board immutable.  I trust that they hold the same values as me, since they are both software crafters.  I trust they both know more about what make great quality code than I do, because they are both crafters.  I trust they know what's best.  But that doesn't stop me wanting to know why.  So...

### Immutable Objects
-----

#### What is an Immutable Object?

An immutable object is one that cannot change.  Its state is defined on creation, and after that point, it cannot be altered.

#### Why should we use Immutable Objects over Mutable objects?

#### Safety

The key benefit of immutabiliy seems to lie in concurrency.  Immutable objects are obviously safe from being altered by different threads.  If a class has a getter and setter method that can be accessed by multiple threads, it is always a possibility that one thread could set an attribute and then wish to read it, but in the meantime another thread has reset that attribute.  So were we to have this class: 

```java

public class Dog {
  
  private String name;

  public Dog(String name) {
    this.name = name;
  }

  public String getName() {
    return name;
  }

  public void setName(String name) {
    name = name;
  }
}

```

If we have mutliple threads utilizing this class, it would be perfectly reasonable for this to happen:

```java

Dog dog = new Dog("Bingo");
dog.getName();    //returns "Bingo"
dog.getName();    //returns "Goofy"

```

Anything relying on a dog named Bingo would break.  Immutability would prevent this.  Immutability ensures thread safety.  I admit here that I do not know a lot about concurrency, and I have not yet used multiple threads so, although I understand the concept here, I'm not sure how it would work in practice.

This isn't, however, just useful with concurrency.  In my Tic Tac Toe I use the negamax algorithm.  This involves my perfect computer player taking the board and playing every possible combination of moves on that board, and then clearing the board afterwards.  There are two issues with this:

1.  The perfect player is affecting the board that the game is used for.  I had a problem with this when I was working on my algorithm, as I feared the computer player was changing the state of the actual game board.  An immutable board would not have this problem.  If every time I made a move I returned a new board, then the state of the board that the game is using would not be altered.

2.  I am doubling the amount of operations that the negamax algorithm has to make.  It fills the board, and then has to clear the board.  Of course, creating a new board every time a move is made has costs of its own, but I will come to that later.  But, ignoring that for the moment, it is obvious that having the negamax doing essentially the same action, once to fill a location with a mark, once to remove the mark, is not efficient.  An immutable board, returned with every mark played, removes this extra cost, since you already have the instance of the board that represents the starting state.

#### Avoid temporal coupling

So how does an immutable object protect against temporal coupling?  Let's start with an example of temporal coupling:

```java

public class Human() {
  
  private Animal pet;

  public String talkToPet(Animal animal) {
    if (!animal.equals(pet)) {
      this.pet = animal;
    }
    return pet.talk();
  }

  public newPet(Animal animal) {
    this.pet = animal;
  }
}

```

```java

public class Dog() {
  
  public String talk() {
    return "woof!";
  }
}

public class Cat() {

  public String talk() {
    return "meow";
  }
}

```

So the talkToPet() method has a hidden side effect.  If we wrote this: 

```java

Human human = new Human();
human.strokePet(new Dog());

```

We would get a null pointer exception when the strokePet() method tried to compare the Animal passed in to the pet.  This code would work: 

```java

Human human = new Human();
human.newPet(new Dog());
human.strokePet(new Cat());   //returns "meow"

```

But the problem is the programmer has to remember to include the newPet() method for the strokePet().  An immutable object would not allow this, thus forcing us to minimize both temporal coupling and hidden side effects, making the Human class easier to test and debug.

These are the two key benefits of immutablity that I see, because I can relate them to my code.  I am sure that there are other benefits, and I have read some, but until I can see the benefits in my code, I am not sure I will fully understand them.

Of course, it is not just class Objects that can be immutable.  I have been reading a little about Rust, which has a strong sense of immutability and uses a system called ownership to maintain immutablity with variables.  It seems really interesting and I'm sure when I can play with Rust a little more, my understanding of immutability will increase.

#### What's wrong with Immutability?
-----

Why wasn't I taught the benefits of immutable objects from the beginning?  Why isn't everyone extolling that all objects should be immutable?  Well, it seems most people are, I just wasn't looking for information on immutability before.  

I remember submitting a weekend challenge at Makers once, in which every single class attribute had an attr_accessor.  This was mainly because my understanding of getters and setters was a bit hazy and I knew if I used attr_accessor I could make things work, but at my code review there was a lot of shaking of heads.  Immutability is a strange concept when you're learning OO.  If you are trying to understand everything as Objects, change is important.  Objects do change.  A Human might want to get a new pet.  It's strange to think that if a human were to get a new pet, you would have to create a new human.  But that's the difference between learning and beginning to understand.  I learnt OO thinking that everything was an object, and now I'm trying to use OO in a way that states that everything is an object, but it is an object in the context of writing good code.

There is also the cost argument that I mentioned earlier.  I've only just started learning about cost, and have never really had to think that much about efficiency before.  The data structures and algorithms course has really pushed me to consider this, which is great.  Logically it makes sense that to update an object would be more cost effective than creating a new one.  I'll quote Oracle on this issue, since I'm sure they've done the research: "The impact of object creation is often overestimated and can be offset by some of the efficiency associated with immutable objects. These include decreased overhead due to garbage collection, and the elimination of code needed to protect mutable objects from corruption.".

So those are my current thoughts about immutability.  I'm sure as my understanding increases and I apply it more in my code I'll have some more insightful things to say but for now, I think that's a fine starting point.

