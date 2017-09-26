---
layout: post
title: Package Principles
date: 2016-05-24 19:00:30 +1000
---

## Packages Principles

This post draws heavily on the Package Principles described in Agile Software Development: Principles, Patterns, and Practices.  The post is split into three parts:  The first is a summary of what I believe to be the key points from PPP, the second part describes how I tried to apply those principles to my Tic Tac Toe and the third is just a note about packages and UML diagrams.

### What is a package?

According to PPP, packages are high level organisation for a project.  In day to day usage, a package is a collection of files, grouped together for ease of use and simplicity in downloading.  When first starting Java, the idea of storing everything in packages seemed strange.  I had not assosciated the idea of a package with the types of packages I was already familiar with and had used, like gems in ruby.  But these are packages. They contain all of the files, classes and code you need in order to run an application.

Even once I had made this connection, however, I was not really considering the principles of packaging code.  If I were to ask myself which classes should belong in which package, I would probably answer all of them, because why would I be writing code that the application did not need?  When it came to my Tic Tac Toe, however, there were a few further questions about which classes should belong where, that needed answering, because there are levels within my Tic Tac Toe.  

I tried to imagine that I was not the author of this code, but really wanted to build a website on which I could play a game of Tic Tac Toe. Perhaps I didn't want to write the logic of winning and losing, and, since someone has already written and packaged this super cool version, I can use that.  But wait, this package contains the entirety of a command line program.  I want to build a web version, so why do I have all these classes relating to the command line?

So I started thinking of where I could divide my Tic Tac Toe into packages, and realised I'd probably need a more concrete approach than wondering what the possible, imaginary, users of my code would want.

### Package Principles in PPP

There are two key concepts to consider when portioning your code into packages:

1. Package cohesion - what classes go in which packages
2. Package coupling - how should these packages be related

### Package cohesion.

#### The Reuse-Release Equivalence Principle
-----


*the granule of reuse is the granule of release*

The quote above sounds really nice, but it took me a really long time to make any sense of it.  So, if like me, you are not sure what the above means, I think it is that the totality of the package released should be reusable, and that the release should encapsulate all of what you want to reuse, so if you want to reuse something, then all you should need is the release.

This means a few things.  PPP talks about the need for a package to be maintained and tracked, because:

1. If the author of the package does not guarantee to maintain the package, this creates more work for the user who would be better off simply writing the package for themselves.
2. If the author does not notify you of a new release and allow you to reject the new release, then the user's code could change without warning and break due to changes the user has not been able to review.

As PPP points out, these demands are mainly political, to do with the relationship with the author of the package more than the package itself, so how can this help decide which classes belong in which package?

*Since reusability must be based on packages, reusable packages must contain reusable classes*

When dividing your classes, then, the needs of the possible reuser must be considered.  PPP gives us two key points on this:

1.  Either all of the classes in a package are reusable or none of them are.
2.  We want all of the classes in a package to be reusable by the same audience.

So, the REP states that we have to consider the users of our code when packaging our code, and that we want to make our packages useful.

#### The Common-Reuse Principle
-----

*The classes in a package are reused together.  If you reuse one of the classes in a package, you reuse them all.*

The CRP is more obviously related to the structure of the application.  Classes that depend heavily on each other, should be in packages with the classes they depend on.  If you want to use a package, you should have the key dependencies of the classes within that package available to you and you should want to use all of the classes within that package.

This is important because:

* If a package contains classes that you do not want to use, if that class needs to be changed by the author, the package that you use will have to be rereleased, you will have to revalidate your code even though you do not care about the changes.

This tells us something important about how to group our classes:

*Classes which are not tightly bound to each other with class relationships should not be in the same package*

#### The Common-Closure Principle
-----

*The classes in a package should be closed together against the same kinds of changes.  A change that affects a package affects all the classes in that package and no other packages*

CCP states that a package should have only one reason to change.  It should have only one responsability.

Just as REP states that we must consider the users and CRP states that we should consider our class dependencies, CCP states that we need to consider maintainability.  If you need to make a change, then that change should only affect one package.  If you want to make a change and find you have to change classes in multiple packages to bring the change to affect, then you will have to revalidate and rerelease multiple packages.

If you have a few classes that are likely to change for the same reason, then they should be packaged together.

This is a principle that I think is definitley easier to adhere to the better your design is.  It is closely linked to both Single Responsability and the Open Closed Principle.  If you have satisfied these principles in your code, then changes should be localised to a few classes.  If you have violated these, then a change one class may ricochet throughout your code, making packaging based on CCP very difficult.

This is also a principle that would be hard to consider if you were trying to organise your code top down.  It is very hard to visualise what a change will affect throughout your code, but throughout a project, as it grows and changes, you become familiar with the patterns of what you have to do in your code depending on what change you make, and where.

### The Principles of Package Coupling

#### The Acyclic-Dependencies Principle
-----

*Allow no cycles in the package-dependency graph*

This principle addresses the issues that can arise when working in a team and helps provide a solution.  A codebase shared between many will be worked on and changed by many.  This means merging new work can be difficult.  Keeping up to date with new code may break previously working code locally.  You may add a new feature based on the code that was up to date when you were working on it, only to find that something you had been depending on has changed in the meantime.

Dividing the code into packages means that this can be avoided, as each package can be a unit of work, and each new change to a package does not have to be adopted by another team immediatley.  This seems a sensible and useful approach, but it relies on one important caveat:  **There can be no cycles**

There will always be dependencies between packages.  It is, as far as I can tell, impossible to work on a shared codebase in isolation.  You will have to, at some point, merge your code with another team's.  Packages help this because each merge can be done incrementally, instead of an attempt to marry all the changes from an iteration at once.

This approach does not, however, work if there are any cyclic dependencies.  If there is a cyclic dependency, the dependencies of those package become instantly more complex.  Say we have two packages, Animals and Habitats.  Habitats depends on Animals, and Animals depends on Habitats.  What problems can this cause?

1. The teams working on both packages have to ensure that they are both constantly working on the most recent version of code.
2. In order to test, you would need a complete build rather than just being able to focus on the package in question.
3. Isolated releases become impossible.

So how do you break a cycle? 

You have two options, using the Dependency Inversion Principle, creating an abstract class or interface, or creating a package to encapsulate the shared dependencies.

#### The Stable-Dependencies Principle
-----

*Depend in the direction of stability*

If you want your application to be useful, then it's going to need to change.  You cannot, and should not want to, have a static application.  Change is a good thing, and you should make it as easy as possible to enact that change.  So whilst you want some packages to be stable and dependable, you need some to be volatile and open to change.

These volatile packages, however, should not have a hard-to-change package depending on it.  If it's changeable, then it is not dependable.  If you have a hard-to-change package depending on it, it, in turn, becomes hard to change.

The easiest way to make something hard to change is to have many things depend on it, because changes to the package will probably create the need for change in all the packages that depend on it.  A package that had many dependents, therefore, is a stable package.

It is good to have some stability, and it is good to have some volatility.  The importance lies in the relationship between those packages.  Stable packages should not depend on volatile packages because you want your volatile packages to be easy change, and you want your stable packages to stay the same. You can ensure this direction of dependency by using the Dependency Inversion Principle.

#### The Stable-Abstractions Principle
-----

*A package should be as abstract as it is stable.*

SAP is concerned with the connection between abstractness and stability.  It states that a stable package should also be an abstract package, so that it's stability does not prevent its extension and that a volatile package should be concrete because it is easy to change.  SAP and SDP together are pretty much the Dependency Inversion Principle for packages, because they ensure that dependencies run in the direction of abstraction.

The issue, however, with comparing package principles with the SOLID principles lies in the fact that SOLID is concerned with classes.  When considering the DIP, you can easily see whether a class is abstract, but a package may have degrees of abstractness.  It could contain some abstract classes, some concrete.


### How does this relate to Tic Tac Toe?


1. The Reuse-Release Equivalence Principle

The essence of Tic Tac Toe is the grouping together of my board, which contains the logic of winning, the game, which deals with switching turns, the player, and the marks. In order for this to be useful, however, you need to be able to play a game, so within that group I would also need the Loop that allows you to play, and the menu which is responsible for the creation of the game.  What I do not need within this group is the players.  Although you do create players in order to play the game, a user may not want the particular players I have created for my implementation of the game, so they are in a different package.  The third package I created was the console package, grouping together all of the specific classes that use the console to communicate with the user.  If, for example, you wanted to use one or both of the other packages to build a web Tic Tac TOe, you would not need this, and so it should not be grouped with the essential elements of the game package.

2.  The Common-Reuse Principle

This was most obvious to implement in the relationship between my GameLoop and my GameMenu.  The menu creates the game that the loop will play.  You cannot have one without the other, and they only work in tandem. But since the Menu creates the Game, it obviously should be in the same package as the Game, which in turn, needs the Board and Marks.  This is the basis of the working game and it depends on all of these classes.

3.  The Common-Closure Principle

I felt a little nervous when I started to implement my 4x4 board.  My board is a very low level class, so I knew that if my design was good, I should not have to make changes all the way up my code structure.  It was interesting to have to implement it after the code base had grown into a fully functioning game and I was relieved when I found there were only a few classes I had to change.  This is the kind of knowledge you gain only when you've been working on your codebase for a while.  Although the change spanned two packages (one to change my board class, the other when changing the display to accomodate the bigger board) it was not the big change it could have been, and I don't believe that the two classes should be grouped together in a package even though if I make a big change to the Board class, such as adding a new size of board, it makes logical sense that the display would have to change also.  This is because, although they are related, and in this case did change together, it would be more likey for the Game to have to change with the board class, and for the reasons above.

4.  The Acyclic-Dependencies Principle

Well, this is a little trickier to apply.  Obviously I want to avoid cyclic dependencies between both my classes and my packages, but since I am the only person working on my Tic Tac Toe, affecting someone else with changes to my code is not a huge concern for me, but it is good practice, so I have made sure there are no cycles in my package structure.

5.  The Stable-Dependencies Principle

PPP has a few formulae for working out the stability and abstraction of your packages.  I did apply it to my package structure as I was working on it, and believe I have satisfied this principle.  I've been trying to apply the DIP to my code, and I think that made it easier, because it seemed quite natural that the more stable package, the game package, would have the most dependents.

6.  The Stable-Abstraction Principle

This I found harder to implement.  I have used DIP so I do have some abstractions, but because my codebase is quite small and there are not a huge amount of dependencies, I haven't had the need to abstract too many classes.  Most of my classes are therefore concrete, and so most of my packages are concrete.  My abstractions, however, after my IPM, were moved to the root package, making it very stable and also very abstract.

#### A note on UML's

Before I started my apprenticeship, I did not know what a UML was.  Now that I know how to represent and communicate the design of my application through UML diagrams, I have found a new medium through which to discovers the flaws in my design. I find design hard, as I have mentioned in this blog, and I'm hoping using package principles may be able to help me.

You can use packages in UMLs to provide a picture of the design with an added layer of abstraction.  Of course, the dependencies you have between classes will remain and will often cross package boundaries, but the relationships between packages are more expressive of the high-level organisation of the application.  This allows you to reason about the design without considering the specifics of each class, which could make it easier to see issues within the dependencies.  

It seems that the design of your application and packaging the classes are connected.  I think the better the overall design of an application, the easier it is to know which classes belong within which packages, and the easier it becomes to partition the code.  As well as this, I think the more familiar you become with the package principles, the easier it is to see when perhaps your design is too complicated.  I think the two ideas can help each other.

This does not, however, mean that we should start designing our applications with the packages in mind rather than the behaviour of our classes.  This kind of top-down design is impossible as the package structure should and will develop at the system grows.  A UML of just packages makes it easier to reason about high-level design because you are not distracted by the function of the actual application and the classes within, but this is the kind of detail that we do want to think about when planning a project.  It is only as the project grows that we need to really consider the dependencies and how to manage them.

So, although packages can be useful with UMLs and assessing design, we cannot and should not be using them for top down design.
