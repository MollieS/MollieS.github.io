---
layout: post
title: Dependency inversion
date: 2016-05-02 13:40:00 +1000
---

## Dependency Inversion

**What is a dependency?**

When something relies on something else

**What is inversion?**

A situation where something is changed so that it was the opposite of what it was before.

## What is the Dependency Inversion Principle?

From PPP:

* high level modules should not depend on low-level modules
* both should depend on abstractions
* abstractions should not depend on details
* details should depend on abstractions


## What does this mean?

### An example


```java
public class Zoo {

  private Lion lion;
  private Penguin penguin;
  
  public void Zoo() {
    this.lion = new Lion();
    this.penguin = new Penguin();
  }

  public String feedAnimals() {
    if (penguin.isHungry()) {
      feedPenguin("Fish");
    } else if (lion.isHungry()) {
      feedLion("Meat");
    }
    return "Animals are fed";
  }

  private void feedPenguin(String foodType) {
    penguin.feed(foodType);
    manage foodStocks(foodType);
  }

  private void feedLion(String foodType) {
    penguin.feed(foodType);
    manage foodStocks(foodType);
  }

  private manageFoodStocks(String animalFoodType) {
    // code to manage stocks of animal food...
  }
}
```

```java
public class Lion {
  
  private String diet = "Meat";
  private boolean fed;

  public void Lion() {
    this.fed = false;
  }

  public void feed(String food) {
    if (food.equals(diet)) {
      fed = true;
    }
  }

  public boolean isHungry() {
    return !fed;
  }
}
```

```java
public class Penguin {

  private String diet = "Fish";
  private boolean fed;
   
  public void Penguin() {
    this.fed = false;
  }

  public void feed(String food) {
    if (food.equals(diet)) {
      fed = true;
    }
  }

  public boolean isHungry() {
    return !fed;
  }
}
```


### Why is this bad?


Dependency Inversion ensures against 3 key issues:

1. High level module, the zoo, depends on low level module, the lion and the penguin.  Any changes to the Lion or Penguin class will affect the Zoo class.  If the Lion decides it no longer wants to eat Meat, the Zoo class will have to change.  If the implementation of the feed function changes, and there is some extra condition added to determine whether or not an animal has been fed, then the Zoo has to know about it.
2. If a high level module is depending on a concrete class instead of an abstraction, it makes it hard to extend.
3. We want our high level modules to be reusable as they will inevitably contain important logic.  Depending on details over abstractions means that our code is not as reusable as it should be.

One of the major concerns of the SOLID principles is the ease in which you can change and add to your code.  Yes, this code will work, as long as the Zoo and the animals stay the same, which is unlikely.

### Hard to change 

#### 1.  Any changes to low level classes will directly affect high level classes.

The Zoo shouldn't be concerned with the intricacies of the feeding methods.  Here it knows the food type of each animal and, were that to change, it would have to change to accomodate them.  It has other responsibilities, managing food stocks and employees.  How to feed the animals isn't it's concern, only that the animals have been fed.

What if an animal were to change?

```java
public class Lion {
  
  private String diet;
  private boolean fed;
  private int age;

  public void Lion(int age) {
    this.fed = false;
    this.age = age;
  }

  private void setDiet() {
    diet = (age > 1) ? "Meat" : "Milk";
  }

  public void feed(String food) {
    if (food.equals(diet)) {
      fed = true;
    }
  }

  public boolean isHungry() {
    return !fed;
  }
}
```

```java
public class Zoo {

  private Lion lion;
  private Penguin penguin;
  
  public void Zoo() {
    // need to add age to Lion
    this.lion = new Lion();
    this.penguin = new Penguin();
  }

  public String feedAnimals() {
    if (penguin.isHungry()) {
      feedPenguin("Fish");
    } else if (lion.isHungry()) {
    // add age condition
      feedLion("Meat");
    }
    return "Animals are fed";
  }

  private void feedPenguin(String foodType) {
    penguin.feed(foodType);
    manage foodStocks(foodType);
  }

  private void feedLion(String foodType) {
    penguin.feed(foodType);
    manage foodStocks(foodType);
  }

  private manageFoodStocks(String animalFoodType) {
  //need to add milk as a food stock
    // code to manage stocks of animal food...
  }
}
```

It's easy to see how any changes to Lion directly affect the Zoo.  This shouldn't happen.  All the Zoo cares about is whether the Lion is fed.
So let's abstract the implementation of fed and see how it helps.

```java
public interface Animal {

  String food();
  void feed();
  boolean isHungry();

}
```


```java
public class Zoo {

  private List<Animal> animals = new ArrayList();
  
  public void Zoo() {
    animals.add(new Lion(3));
    animals.add(new Penguin());
  }

  public String feedAnimals() {
    for (int animal = 0; animal < animals; animal++) {
      if (animal.isHungry()) { 
        animal.feed(animal.food())
        manageFoodStocks(animal.food());
      }
    }
    return "Animals are fed";
  }

  private manageFoodStocks(String animalFoodType) {
    // code to manage stocks of animal food...
  }
}
```

So now, instead of depending on each Animal, the Zoo depends on the interface, which promises to the Zoo that each animal will be able to tell it if it hungry, and knows how to be fed.  The Zoo doesn't care which animal is which, just as long as they are fed.

Is this code now satisfying the Dependency Inversion Principle?  Let's look again at the criterea from PPP.

1.  High level modules should not depend on low-level modules.  Both should depend on abstractions.
The Zoo no longer depends on the concrete classes of Lion or Penguin in the implementation of feeding the animals.  It is not, however, fully free of it's dependency from the Lion and Penguin class, but we'll get to that momentarily. So, although we're closer than we were before, we're still not fully satisfying DIP here.
2. Abstractions should not depend on details.  Details should depend on abstractions.
This, I feel, we have satisfied in this context.  Before, the zoo cared about which animal was which and knew how to feed them accordingly.  We have removed those implication details from the Zoo.  The details lie only in the implementaton within the classes that use the interface.  Any changes to the diet, the feed method or the isHungry method will affect only the class it is implemented in.

A side note: This is starting to sound a little like OCP, but I'll ignore OCP for the time being, because I'll come back to it.

#### 2.  Hard to extend.

A new animal!

```java
public class Zebra implements Animal {

  private String diet = "Grass";
  private boolean fed;
  
  public void Zebra() {
    this.fed = false;
  }

  public void feed(String food) {
    if (food.equals(diet)) {
      fed = true;
    }
  } 

  public boolean isHungry() {
    return !fed;
  }

  public String food() {
    return diet;
  }
}
```

```java
public class Zoo {
 
 private List<Animal> animals = new ArrayList();

  public void Zoo() {
    animals.add(new Lion(3));
    animals.add(new Penguin());
    // should not need to change this.  What if we had five new animals?
    animals.add(new Zebra());
  }

  public String feedAnimals() {
    for (int animal = 0; animal < animals; animal++) {
      if (animal.isHungry()) { 
        animal.feed(animal.food())
        manageFoodStocks(animal.food());
      }
    }
    return "Animals are fed";
  }

  private manageFoodStocks(String animalFoodType) {
    // code to manage stocks of animal food...
  }
}
```

So, here, we can see the dependency that Zoo still maintains on the concrete classes.  This happened earlier as well when adding the age to the Lion.  This violates DIP, but the way to solve it is with dependency injection.  So now the Zoo can depend on the abstraction rather than the concrete classes.

### 3.  Reusable.

One of the key values of code that adheres to DIP is that the high level modules should be reusable, so let's test that theory.  The example above violates DIP and we can also see how that violation makes the code less reusable.  What if we wanted to create another zoo which had other animals?  Because of the dependency within Zoo, the important logic of managing food and all the other things a zoo might do, would have to be replicated instead of us being able to easily reuse it.

But dependency injection has solved this problem for us:

```java
public class Zoo {
 
 private List<Animal> animals = new ArrayList();

  public void Zoo(List<Animal> zooAnimals) {
    animals.addAll(zooAnimals);
  }

  public String feedAnimals() {
    for (int animal = 0; animal < animals; animal++) {
      if (animal.isHungry()) { 
        animal.feed(animal.diet)
        manageFoodStocks(animal.diet);
      }
    }
    return "Animals are fed";
  }

  private manageFoodStocks(String animalFoodType) {
    // code to manage stocks of animal food...
  }
}

// Zoo smallZoo = new Zoo(new Penguin(), new Zebra());
// Zoo bigZoo = new Zoo(new Tiger(), new Leopard(), new Giraffe(), new Rabbit());
```

## OCP and DIP

OCP and DIP differences lie in the scope of the principles.  OCP focusses on one class or module, and, although you may need to create abstractions in order to satisfy it, you're still only considering the one class when doing so.  DIP is focussed on the dependencies between modules and classes, and the intraction between them.
