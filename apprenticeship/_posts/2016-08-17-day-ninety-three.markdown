---
layout: post
title: Day ninety-three
date: 2016-08-17
---

The Android Software Stack
---------------------

Android is a software stack all of its own with four seperate layers:


Linux Kernel
------------------------

The lowest level of the Android stack is the linux kernel.  It provides much of the same functionality a regular linux kernel would, so provides a permissions architecture ensuring security, memory and process management, file & network IO, and device drivers.  But the mobile device is fundamentally different from a desktop computer, so the Android linux kernel has some extra features:

1. Power Management
2. Memory sharing
3. Low memory process killer
4. Interprocess Communication

These features are specific to the Android kernel to deal with issues uniquely faced by mobiles.  Power management is needed as mobile devices will run on battery.  Memory sharing is required because a mobile device will have a limited amount of memory and also will not have a powerful CPU.  The kernel can kill processes when memory is low in order to preserve it, and the Binder Interprocess Communication allows seperate processes to communicate with each other.

System/Native Libraries
------------------------------

The Android system libraries are written in C/C++.  They provide core functionality, like mathmatical computation and memory allocation, as well as:

* Media framework for images and music
* Webkit for websites
* SQLite
* OpenGI

Within this layer is the Android Runtime, which contains:

* The core Java libraries
* The core Android libraries
* JUnit
* org libraries to use the web

And it also contains the Dalvik Virtual Machine.

Although Android uses Java, it executes programs within the Dalvik Virtual Machine rather than the JVM.  The source code is written in Java, compiled with a Java compiler into bytecode, converted to Dex bytecode and then packaged and installed.

Android uses DVM over JVM because of the unique constraints on a mobile system.  The DVM was designed with slower CPUs, limited RAM and limited battery life in mind.

Application Framework
--------------------------------

The Application Framework provides reusable software that might be needed for mobile applications.  It includes:

1.  Package Manager - a database that tracks all the apps on a device.  The package manager stores information about applications allowing them to find and contact each other.
2.  Window Manager - manages the windows of applications.
3.  View System - includes UI elements that applications may need such as buttons
4.  Resource Manager - keeps track of non-compiled elements that applications need, for example strings.  This allows languages to be easily changes as the string values are not hard coded into applications.
5.  Activity Manager - manages application lifecycles and navigation stack.
6.  Content Provider - stores content so that it is available to multiple applications and can be shared between them.
7.  Location Manager - manages location and movement
8.  Notification Manager - manages notificiations.

Applications
--------------------

The top of the Android stack is the applications.  In Android, none of these are hardcoded, meaning that all of them can be replaced, deleted and added.  They can also be added not just through the play store.
