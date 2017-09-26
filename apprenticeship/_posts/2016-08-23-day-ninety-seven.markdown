---
layout: post
title: Day ninety seven
date: 2016-08-23
---

Android, Java, Jack and Jill
----------------------

I have had some troubles with Android, Java and Travis.  These three should get on.  Android and Java should get on perfectly, Java and Travis work, so why should they not all work together?

I have had some issues with them all, and I have learnt some things along the way.

Android and Java
------------------

The Android version about to be released is Android 7, Nougat.  This version uses the Android API 24, which currently uses the build tools version 24.0.1.  It makes sense to make this my target SDK.  I don't want to write an application optomized for an out of date platform.  So that's what I did.  The build tools need the JDK to be 1.8 as, I suppose, it uses some Java 8 features.  This is fine.  When I imported my Tic Tac Toe jar as a library, everything was fine.

The only problem with using Nougat is that it is only just out, so not many people can actually play the game on their phone.  It took me a while to even find an emulator that I could run it on.  This isn't fine as someone, someday in the near future, may want to play Tic Tac Toe on their phone.  So I lowered my minimum SDK to 23, compatible with Marshmallow, Android 6.  This could have been fine, but Android 6 is not compatible with Java 8, and my Tic Tac Toe jar used some Java 8 features, so Android wouldn't compile it.

I changed the jar to be Java 7 friendly and everything was fine, until I started integrating with Travis.

Travis and Android
--------------------

I had a few teething problems getting Android and Travis to work together, mainly just specifying the whereabouts of some of the libraries I needed.  I finally got that working and the gradle build began, but my build was still failing before it ran the tests.  

Travis exited with this message: `The command "./gradlew build connectedCheck" exited with 137` during the gradle command `:app:compileReleaseJavaWithJack/home/travis/build.sh`.  This was the beginning of my adventure finding out who Jack was.  He had appeared earlier on in the project, but I had more or less ignored him.  Now he was making sure I couldn't ignore him any longer.

In order to use Android API 24, I had changed my build.gradle file to include the sourceCompatability and targetCompatability to be Java 1.8.  In order for this to work, gradle told me I needed to enable jackOptions.  I did this, everything worked, I ignored it.  I do not know the intricate details of gradle, usually I don't really need to, so often, if it tells me I need to do something, I just do it and move on.

Now though, I wanted to find out who this mystical Jack was and why he was appearing in my code.  Googling the error I got from Travis gave me exactly 0 results in as many variations I could word it.  So...

Who is Jack?
--------------------

I already knew that Jack had something to do with Java 8, and looking to the Android docs I found some answers.

As I mentioned in a previous post, Android compiles Java and then convert the bytecode into DEX bytecode.  So we have:

Javac(.java -> .class) -> dx(.class -> .dex)

But this doesn't work for Java 8, so Android uses Jack, which works like this:

Jack(.java -> .jack -> .dex)

The Android documentation says that the good thing about Jack is that it compiles faster, because the .jack file format contains precompiled dex code.

Jack also comes with Jill, which translates .jar files to .jack files, so that they can be integrated fasted with a project.

As far as I can tell, this is what Jack does, and I cannot use Android 24 without him.

Jack and Travis
---------------------------

The bad news is that Jack is experimental, and it could be what is making my Travis builds fail, although I'm not completely sure.  I did, eventually, get my build to pass, but it only runs half of my test suite.  By changing the default `./gradlew connectedCheck` command to the `./gradlew test` command, the build passes, but it does not run my espresso tests, only my JUnit tests.  The espresso tests use an emulator, so it could be this that is failing my build.  The difficulty I have is that I have no error message.  It simply kills the build during the compileReleaseJavaWithJack command.  I am currently assuming that this command is necessary to create the .apk to install onto the emulator to run the tests, which is why the build fails with the emulator tests, but finding out what this command actually does is proving difficult.  I am not 100% sure about this though, but after spending a whole morning trying to get it working, I decided that a break wouldn't do me any harm and I began work on my menu instead.

If this is the problem, I'm not sure how to fix it.  I am not going to target an older version of Android just so that my Travis build passes with all of my tests, and I have tried asking Travis to use a lower version of the Android API, but I haven't managed to get it working yet.  I don't know how I can write acceptence tests that don't require an .apk to be built, so I'm at a bit of a loss at the moment, but hopefully when I go back to it things will be clearer.  What is currently frustrating is that I haven't been able to find many answers.  I am going to go ahead and assume I am not the only person developing an application for Android 7, and I'm also going to assume I'm not the only person testing their application, and I am also going to assume that I am not the only person having this problem with Travis, so why is the answer so elusive?

The rest of the app is making good progress though.  I'm now using Intents to start multiple activities and I'm going to start adding extras to those intents which, I hope, will act as parameters to trigger the creation of the correct game type.
