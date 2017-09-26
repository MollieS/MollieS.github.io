---
layout: post
title: Day ninety-one 
date: 2016-08-15 14:40:00 +1000
---

Android Basics: Activities, Services and Processes
-------------------

I've been doing the android course on udacity the past couple of days, and am trying to make sure that I'm really understanding what I am learning.  Many of the key concepts are introduced and briefly explained in the videos, which so far have been very informative, but I have found the android documentation to be really good, which has made it really easy to research some of the core concepts further.  This post is mainly a collection of my notes on Activities, Services and Processes, but these cannot be understood with a basic knowledge of how Intents and Threads work in Android, so I have included the basics of those concepts as well.  All of these subjects have a lot more to them than what I have described below, which I hope to explore when I need to.

### Intents
------------------

Intents are a core element of an Android application structure.  They provide an abstract description of the operation that is needed.  They can be used to start an Activity, Service and many other application components.  They are made of two core parts: the action required and the data that is needed to perform that action.

There are two types of Intent:

1.  Explicit - an explicit intent has a specified component providing the exact class to be run, and because of this, does not need any more information
2. Implicit - an implicit intent does not specify a component, and so it has to include enough information for the system to work out the best component to complete the operation required.


### Threads
--------------------

The most important thing to understand about threads in Android is the main thread, also called the UI thread.  When an application launches, it creates the main thread as the single thread of execution.  This thread is in charge of all UI activity.  It is concerned with how the user interacts with the application, including any events that take place, and the widgets that those events trigger or communicate with.  So it is the main thread that ensures that the application acts as expected from the users point of view, and it is the main thread that keeps the UI functioning.

The Android system does not create new threads for each component, and all componenets that run in the same process are sustained by the same thread.

This is important because unless seperate threads are created to deal with long running processes, then the UI will be blocked.  It is also important that the UI thread is the only thread that deals with the UI, as it is from the main thread that all system calls are dispatched.  The Android documentation gives two rules for the single thread model:

1.  Do not block the UI thread
2.  Do not access the UI from outside the UI thread

### Activities
---------------

An Activity is a core class in Android.  It represents a single, focused thing that a user can do.  Activities are concerned with the interaction with the user, and these are the classes that create the windows into which you can place your UI.  They can be full screen, but they do not have to be.  Android supports windows embedded within others and floating windows.

Activies also support fragments, which can be specific, modularized parts of the UI.

#### Activity Lifecycle

Activities are managed in a stack.  When a new activity is started, it becomes the running activity, pushing the previously running activity into the stack below it, so that that activity will only be used again once the new one exits.

Every activity has four states:

1.  Active or running
2.  Paused: it is no longer in the foreground.  A paused activity is still alive and maintains all of its state, but can be killed if memory is very low.
3. Stopped: an activity is stopped when it is completely obscured.  It remains active, and maintains state, but can be killed if memory is needed.
4. Dropped: if an activity is paused or stopped, the system can drop it by asking it to finish or by killing it.  If and when it is displayed again, it must be restarted and state must be restored.


These states are reflected in the methods an Activity has:

1. onCreate()
2. onRestart() && onStart()
3. onResume() && onPause()
4. onStop()
5. onDestroy()

The activity is only killable if it has been stopped or destroyed and, pre Honeycomb, when paused.  This means that the application's exit strategy should prepare itsel as early as when `onPaused()`.  Of course, when an activity is paused, it is ususally still visible, so the UI should be updated.

So, an activity lifetime is as follows:

* entire lifetime: from when `onCreate()` is called to when `onDestroy()` is called.
* visible lifetime: from when `onStart()` to `onStop()` is called, which can be multiple times as an activity is hidden or visible.
* foreground lifetime: between the call `onResume()` and `onPause()`, which can happen frequently.


### Services
-------------------

Services are components that can be created to perform long running processes, as they do not provide a user interface and so should not interfere with the user experience.

There are two forms of services:

1.  Started:
    * A started service is created when another application process, like an activity, calls `startService()`
    * It can run in the background indefinitley, even after the component that started it is destroyed
    * It does not usually return a result
    * It should stop itself once its task is complete
    * A component started with `startService()` remains running until it calls `stopSelf()` or another component calls `stopService()`, and so it is the developer's responsibility to stop the service.

2.  Bound:
    * An application component can bind itself to a service with `bindService()`
    * It can provide a client-server interface, allowing components to interact with it
    * It can send requests and get results
    * It can interact across processes (Interprocess Communication)
    * It runs only as long as the component it is bound to exists
    * Many components can bind to it, but once it is unbound, it is destroyed

It is important to note that one service can behave as a started and a bound service, and, especially important for a started service, the system will only force stop a service when memory is low.

Services do not, by default, create their own thread.  They run in the main thread of its hosting process, so if that is the UI thread and the service will run a long running task, then a new thread should be created for it.

#### Intent Services

Intent Services are a subclass of the base Service class. They are created with an Intent, and are good for a few reasons:

1.  An Intent Service creates a default worker thread to execute all Intents seperate from the main thread.
2.  It creates a work queue, protecting against mutlithreading dangers (The base Service class can hadnle multithreading if it is needed)
3.  It stops itself after all of its requests are handled

#### Service Lifecycle

* entire lifetime - for all services, bound or not, the entire lifetime lasts from `onCreate()` until `onDestroy()`
* active lifetime - lasts from `onStartCommand()` or `onBound()` until the entire lifetime ends.  It is destroyed when stopped.

### Processes
-----------------

The Android system attempts to keep application process running as long as possible, and the decision about which process to kill is ties to the state of the user's interaction with it.  By default, all components run in the same process, but this can be altered.

The system judges which processes to kill based on the 'importance hierachy' which is as follows, starting with the most important:

* Foreground Process
  * Hosts an activity in the foreground
  * Hosts a service that is bound to an activity that a user is interacting with
  * Hosts a service that is running in the foreground, e.g when `startForeground()` is called
  * Hosts a service that is executing one of its lifecycle callbacks, e.g `onCreate()`, `onStart()` or `onDestroy()`
- only a few foreground processes will exist at one time
- if it has to be killes, the device will have reached memory paging state, so the foreground process must be killed to keep the UI responsive

* Visible Process
  * Does not host any foreground components, the componenet it hosts can affect what the user sees on screen
  * Hosts an activity not in foreground, but still visible, e.g when `onPause()` as been called
  * Hosts a service bound to a visible or foreground activity
- will only be killed if the memory it uses is needed to keep the foreground processes alive

* Service Process
  * Hosts a service that has been started with `startService()`
  * Hosts a service that is not directly tied to what the user sees, but doing something that the user cares about, e.g downloading data or playing music

* Background Process
  * Hosts an activity not currently visible, e.g `onStop()` has been called
  * Hosts an activity that does not directly impact user experience
- There can be many background processes running at once, and they are kept in a least recently used list, so that the most recently seen is the last to be killed
- If a process is killed and the user navigates back to it, the activity restores all of its visible state

* Empty Process
  * Hosts no active application componenets
  * Mainly used for caching

There are some other factors that increase the importance of a process, the key one being that other processes may depend on it.
