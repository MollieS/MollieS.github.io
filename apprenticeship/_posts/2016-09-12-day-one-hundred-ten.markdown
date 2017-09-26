---
layout: post
title: Day one-hundred and ten
date: 2016-09-12
---

Async Tasks
------------------

I finally got around my issue of blocking the UI Thread in my Android app, and the answer lay with Async Task, so I thought I could write about the basics.  

The Problem
----------------------

What is the point of AsyncTask and should we use it?

Here's an example:

```java

public class AsyncExampleActivity extends AppCompatActivity {

  private Button mSleepButton;
  private Button mToastButton;
  private String LOG_TAG = "AsyncExampleActivity"

  @Override
  public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.id.activity_async_example);
    
    mSleepButton = (Button) findViewById(R.id.sleep_button);
    mToastButton = (Button) findViewById(R.id.toast_button);
    
    mSleepButton.setOnClickListener(new SleepButtonListener());
    mToastButton.setOnClickListener(new ToastButtonListener());
  }

  private class ToastButtonListener implements View.OnClickListener {
    
    @Override
    public void onClick(View view) {
      Toast.makeText(getApplicationContext(), "This is a Toast!", Toast.LENGTH_SHORT).show();
    }
  }

  private class SleepButtonListener implements View.OnClickListener {
  
    @Override
    public void onClick(View view) {
      try {
        Thread.sleep(3000);
      } catch (InterruptedException e) {
        Log.e(LOG_TAG, e);
      }
    }
  }
}
```

So, there's a lot of code going on here which I will try to go through from the top.

First we override the `onCreate()` method.  This is inherited from AppCompatActivity and is called when the Activity is created.  We have to refer to the super method, and then we declare the id of the layout file we wish to show.

Then we look in the view to find two buttons, and set OnClickListeners on them, which are defined below.

We have the Toast button, which shows a Toast, which is a small popup that includes some text and will remain visible for a set amount of time.  Then we have the Sleep button, that sleeps for 3 seconds.

If we were to run this code, when the Toast button is pressed, a Toast would show, and if we were to press the Sleep button, it would sleep.  The problem is that if we were to press the sleep button and try to press the Toast button straight afterwards, nothing would happen for 3 seconds.  The UI would be frozen.  This is because of the Single Thread Model of Android.

This isn't great, especially if we imagine that instead of sleeping, we wanted the sleep button to carry out a task that took three seconds, like retrieve data from an external API.  This is where we need an AsyncTask, so we can create one to carry out our long running task on another Thread, allowing the main thread to keep the UI functioning.

The Solution
-----------------------------------

```java

private class SleepTask extends AsyncTask<Void, Void, Void> {

  @Override
  public Void doInBackground(Void...voids) {
    try {
      Thread.sleep(3000);
    } catch (InterruptedException e) {
      Log.e("ERROR", e);
    }
    return null;
  }
}
```

We've moved the sleep task to the AsyncTask and asked it to run in the background.  The signature of this method can change depending on the `<Void, Void, Void>`  The first argument here is the expected parameter type to the `doInBackground()` method, the second is the type in which progress is measured, and the third argument is the type which you expect to be returned from the `doInBackground()` method.

To use this new AsyncTask, the sleep button listener would now look like this:

```java
private class SleepButtonListener implements View.OnClickListener {

  @Override
    public void onClick(View view) {
      new SleepTask().execute();
    }

}
```

With this code in place, once the sleep button is pressed, we can still press the Toast button and have a toast pop up show.

Complications
-------------------

One of the golden rules of Android is to never block the UI thread.  We've sorted that with the AsyncTask.  Another golden rule of Android is that we should not access UI Elements from
outside of the UI thread.

Why would we want to do this?  Imagine we have added a progress bar to the layout.  When the sleep button is pressed, we make this progress bar visitible for the duration of the time it takes to carry out the sleep, and then we make it disappear.  To do this, it would almost make sense to put this code as shown below, or at least it made sense to my brain that wasn't quite used to asynchronisity when I started using AsyncTask:

```java
private class SleepButtonListener implements View.OnClickListener {

  @Override
  public void onClick(View view) {
    mProgressBar.setVisibility(View.VISIBLE);
    new SleepTask().execute();
    mProgressBar.setVisibility(View.INVISIBLE);
  }
}
```

Here, I'm saying "set the progress bar to be visible, run this task, then set it not invisible", but obviously it doesn't work.  `execute()` doesn't just say "do this thing".  It says "ok, you can start working on this thing and I'm going to carry on as usual."  so the linear progression of a method that I'm used to is not what's happening here.  What's happening is that once `execute()` is called, the progress bar once again becomes invisible.  To make it show for the duration of the task, we're going to have to make it invisible in the only place that knows how long the task takes, which is in the Background thread.


If we were to try this, however:

```java
private class SleepTask extends AsyncTask<Void, Void, Void> {

  @Override
  public Void doInBackground(Void...voids) {
    try {
      Thread.sleep(3000);
    } catch (InterruptedException e) {
      Log.e("ERROR", e);
    }
    mProgressBar.setVisibility(View.INVISIBLE);
    return null;
  }
}
```

This says that after we have finished sleeping, we want the progress bar to disappear, but we would get an error.  The code is in the right place now, but it is in the wrong thread.  Android will give us an error about trying to interact with UI elements from outside to the main thread, but we can fix this!  All we need is access to an Activity.  In this case, our SleepTask class is a private class of the AsyncExample class, so we have the Activity and all its methods available, but Android provides many different ways to access the host activity from outside as well, because they are so useful and powerful.

```java
private class CarryOutTask extends AsyncTask<Void, Void, Void> {

  @Override
  public Void doInBackground(Void...voids) {
    try {
      Thread.sleep(3000);
    } catch (InterruptedException e) {
      Log.e("ERROR", e);
    }
    runOnUiThread(new Runnable() {
      @Override
      public void run() {
        mProgressBar.setVisibility(View.INVISIBLE);
      }
    });
    return null;
    }
}
```

And now all our code works!  If we press the sleep button the progress bar appears, and whilst it is spinning we can still press the Toast button to display a Toast.


