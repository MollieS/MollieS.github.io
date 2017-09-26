---
layout: post
title: Media Playback
date: 2017-09-26
tags: android
---

# Android Media Playback
-------------------


Media Playback in Android is not the most difficult concept to grasp.  Like many other things, the Android system makes it relatively simple to create an application that plays sound.  The real issue is how to do it right.  How can we be responsible when it comes to memory?  How can we be responsible when it comes to interacting with the system as a whole?  How do we deal with receiving phone calls? Or notifications?  These are the issues that make you a responsible Android developer.

Before we can discuss how to manage these elements, let's take a step back to cover the basics

### Playing a song
-------------------

![initial layout]({{ site.url }}/assets/android-media-playback/layout.png)

We have an initial layout, a blank screen with a play button.  How do we go from that into a song being played?

First there needs to be sound to play.  This can be done a few ways, but the simplest is to include a sound asset.  For an android project the sound file must exist in the res/raw directory.

![file-structure]({{ site.url }}/assets/android-media-playback/file-structure.png)

From here, a sound file acts as any other res file and can be accessed in the same way.

The MediaPlayback class
