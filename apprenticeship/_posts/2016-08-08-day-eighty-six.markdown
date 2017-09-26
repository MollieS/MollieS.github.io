---
layout: post
title: Day eighty-six
date: 2016-08-08 16:40:00 +1000
---

Passing functions in Java
-------


I had some duplication in my Pokemon Manager that I couldn't work out how to fix, and Felipe helped me work out how to do it, so I thought I'd blog about it.

The duplication was in the class that dealt with communicating with the database.  The basic layout for a method was this:

```java
public void save(String name, String height) {
  Connection connection = getConnection(); // connect to database
  Statment statement = connection.createStatement(); // create a statement
  String sql = "INSERT INTO POKEMON (name, height) VALUES " + name + "," + height + ";";
  statement.execute(sql); // do something to the database
  connection.close();
  statement.close();
}
```

As I added more methods, I refectored them to look more like this:

```java
public void save(String name, String height) {
  Connection connection = getConnection(); 
  Statment statement = connection.createStatement(); 
  savePokemon(name, height, statement); // generalize the method called
  connection.close();
  statement.close();
}

public List<Pokemon> getPokemon() {
  Connection connection = getConnection(); 
  Statment statement = connection.createStatement(); 
  List<Pokemon> pokemon = getPokemon(statement);
  connection.close();
  statement.close();
}
```

Which made it easier to see the pattern in my methods.  The problem was that the thing that was changing in each method was the action required, and I had no idea how to pass that action into a method.

Felipe told me about java Function.  I hadn't used it before, but it allows you to pass a function into a method as an argument, as long as you specify what the function takes in, and what it passes out, so I could do something like this:

```java
private <T> T runInConnection(Function<Statement, T> operation) {
  Connection connection = getConnection();
  Statement statement = connection.createStatement();
  T returnValue = operation.apply();
  connection.close();
  statement.close();
  return returnValue;
}
```

So now I can pass in the methods that actually execute what I want, and to do this, I used lambdas.

```java
public void save(String name, String height) {
  runInConnection(statement -> savePokemon(name, height, statement));
}

public List<Pokemon> getPokemon() {
  return runInConnection(statement -> getPokemon(statment));
}
```

So I can call the method, pass the statement in, and leave the resposibility of creating a connection and closing a connection in one place, instead of having to recreate the connection in every method that needs to communicate with my database.
