---
layout: post
title: Day fourty-three
date: 2016-06-03 16:40:00 +1000
---

Single Reponsibility Principle
------

The Single Responsibility Principle states that each class and interface should have only one reason to change.  Each resposibility is an axis of change and, should a class have to change for whatever reason, that change should only affect that responsibility.  If the principle is violated it means that, when change occurs, you will have to retest or redeploy code uneccessarily.

With OO, I feel the name can be misleading.  Since we base our objects on real world examples, it's quite easy to assosciate a classes responsibility with the object itself.  This can be helpful when designing classes, as we get a feeling of what should be responsible for what, but it can also lead us to having our classes do to much.

Say we were to have this interface:

```java
public interface Console {
  
  public void getName();

  public void printGreeting();

  public void printPrompt();

}
```

It makes sense that these methods should all be grouped together.  We have a console that deals with recieving input and printing output.  We would implement these methods as follows:

```java
public class Greeting implements Console {

  private String name;

  public void getName() {
    Scanner scanner = new Scanner(System.in);
    name = scanner.next();
  }

  public void printGreeting() {
    System.out.println("Hello " + name);
  }

  public void printPrompt() {
    System.out.println("What is your name?");
  }
}
```

This, again, seems to make sense, but what if we wanted to change our greeting class to print a language other than English?  What would we have to change?  Both of the print methods would change, but the functionality for getting input would not.

This is a trivial example, but if we had a lot more print statements that we needed to change, and if we had validity checks for different kinds of input, the differences between the two responsibilities become clear.  It would be better to have this:

```java
public interface ConsoleDisplay {
  
  public void printGreeting();

  public void printPrompt();
}
```

```java
public interface ConsoleInput {
  
  public String getName();

}
```
So we can see that the only responsibility of this interface is to display messages to the user.  We could then have a FrenchDisplay and an EnglishDisplay, and the input would not have to change:

```java
public class FrenchDisplay implements ConsoleDisplay {
  
  public void printGreeting(String name) {
    System.out.println("Bonjour " + name);
  }

  public void printPrompt() {
    System.out.println("Comment t'appelles-tu?");
  }
}
```
```java
public class Input implements ConsoleInput {
  
  public String getName() {
    Scanner scanner = new Scanner(System.in);
    return scanner.next();
  }
}
```

If the Single Responsibility Principle is violated, the code will generally be rigid and hard to change, but there is a danger in attempting to adhere.  Because the SRP is based on change, it can only really be judged in regards to when change happens.

If there is no change, there is no axis of change, and so SRP is almost nullified.  Seperating your responsibilities if it is not in response to change could lead to needless complexity in your code.  So, as PPP states: "Do not apply a principle if there is no symptom".

Sometimes we are forced to couple things.  We cannot write code in complete isolation, and it could be something like the operating system that forces us into coupling, but we can decouple concepts as far as our application is concerned.  If, however, we cannot decouple concepts, we must make sure that our dependencies flow away from our coupled class, to ensure that change is as easy as possible.
