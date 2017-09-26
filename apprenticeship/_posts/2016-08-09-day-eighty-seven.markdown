---
layout: post
title: Day eighty-seven
date: 2016-08-09 16:40:00 +1000
---

SQL Injection
----------------

SQL Injection is a type of Code Injection, which exploits a weakness in an application by providing invalid input.  SQL Injection does this by passing input that would affect a database.

You are vulnerable to a SQL Injection if you have sql commands that depend on user input, and there are no validations on the user input before it is inserted into the sql command and executed.

So, in PokemonManager, I have something similar to this in order to free my pokemon.

```java
private void delete(String name, Statement statement) {
  String sql = "DELETE FROM POKEMON WHERE NAME = " + name + ";";
  statement.executeUpdate(sql);
}
```

In my core package, there are no validations on what this name is, so it could be something like:

```java
String maliciousString = "'pikachu' or 0=0";
```

When this is passed to the delete method, the sql String would end up looking like this:

```java
private void delete(String name, Statement statement) {
  String sql = "DELETE FROM POKEMON WHERE NAME = 'pikachu' or 0=0;";
  statement.executeUpdate(sql);
}
```

If this sql was executed, it would delete every pokemon in the database, since 0=0 will always be true.  The implications of being able to execute user defined sql commands are wide.  There may be sensitive data in a database, like passwords or credit card information, which could be accessed with a simple 'SELECT * FROM USERS' added to the end of an input.  Because multiple sql commands can be executed by a single statement, you can chain sql commands.  So in the above, you could change the maliciousString to look like this:

```java
String maliciousString = "'pikachu'; SELECT * FROM USERS; DROP TABLE USERS";
```

Which would give the author of the string all the information of users, and then delete all the user information.

Attacking Pokemon Manager
------------------------

At the moment, although my core is vulnerable to SQL Injection, I don't think it is possible from the CLI app.  I have tried, and failed, but then again, I've never tried to hack anything before so maybe I'm just not very good at it.  I think, however, it can't be done in PokemoManger, because the CLI app validates the input of the user.  There are two main database queries that require user input.

  1. Delete - CLI app checks to see whether the name a user inputs corresponds to a caught pokemon and then, instead of passing the user inputted information to the database manager, passes the name of the pokemon, I haven't been able to find a work around to wipe the database.
  2. Insert - Similar to delete, the CLI app creates a pokemon object to be saved to the database, and it is the details of the pokemon that are inserted into the SQL commands, not the user inputted information, so I haven't managed to break this one yet either.

Currently thinking about what affect SQL Injection could have on Pokemon Manager is trivial, since it is not a web app and all data is stored locally.  If a user decided to wipe the database, they could more easily use psql to drop the table, and it would be only their data that is lost.  The problem would come if the core was used to create a web application.

For a web app, the database would be centralised so that it could serve multiple users, and so a malicious SQL query could affect the entire database.  Anyone using the core package would have to know that the core was vulnerable to SQL Injection, and they would have to ensure that they protected against it.

