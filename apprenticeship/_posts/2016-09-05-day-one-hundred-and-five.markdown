---
layout: post
title: Day one hundred and five
date: 2016-09-05
---

How To Maintain Information When You Rotate Your Phone In Android
------------------------------

I wrote in a [previous post](https://mollies.github.io/2016/08/15/day-ninety-one.html) about Activity Lifecycles, and their four states.  What I have been learning about since then is how to keep hold of information through the lifecycle.

Because Android does its best not to kill processes and stop Activites, in general this is pretty easy.  You can pass information between Activities with Intents, using extras, and whilst an Activity is active, it will keep hold of any data you have given it.

A funny thing about Android, though, is that when you rotate your screen, it calls `onDestroy()` and then calls `onCreate()`, so that when you rotate your device, you are actually creating a new Activity.  I'm not entirely sure why this happens. By default, Android will use the protrait layout for the landscape page, but you can create custom landscape layouts which would require the activity to reload with the new layout, so that might have something to do with it, but I need to investigate this further.  Anyway, this is what happens, and as a result any data that your Activity had recieved is then lost. 

Here's an example.  Let's say we have an Counter Application.  It shows a number, starting from zero, and then counts up when a button is pressed.  The Activity would look something like this:


```java
public class CounterActivity extends AppCompatActivity {

  private int mNumber;
  private Button mIncreaseButton;
  private TextView mNumberDisplay;

  @Override
  protected void onCreate(Bundle savedInstanceState) {
      super.onCreate(savedInstanceState);
      setContentView(R.layout.activity_counter);
      setNumberDisplay();
      setIncreaseButton();
  }

  private void setNumberDisplay() {
    mNumber = 0;
    mNumberDisplay = (TextView) findViewById(R.id.numberDisplay);
    updateView();
  }

  private void setIncreaseButton() {
    mIncreaseButton = (Button) findViewById(R.id.increaseButton);
    mIncreaseButton.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View view) {
          ++mNumber;
          updateView();
        }
    });
  }

  private void updateView() {
    mNumberDisplay.setText(String.valueOf(mNumber));
  }

}
```

As long as this Activity remained alive, whilst the button was pressed, the number on the screen would increase.  If you were, however, to rotate the screen, then the number would reset to its initial state of zero.  The code would still work, and the number would increase again in landscape, but if it were rotated again, it would again reset to zero.

Android does allow us to maintain that state though.  The `onCreate()` method takes a Bundle parameter, which, as far as I can tell so far, is just a bunch of state.  We can edit what is passed into the Activity by overriding another Activity method, `onSaveInstanceState()`.  This is the method that is called when an Activity is called, and where the data that Android saves about the Activity is saved.  So we can access that Bundle and input some of the data we wish to keep hold of.  The Bundle saves data in the form of key value pairs, so we can do this:

```java

private static final String CURRENT_NUMBER = "CurrentNumber";

//...

@Override
public void onSaveInstanceState(Bundle savedInstanceState) {
  super.onSaveInstanceState(savedInstanceState);
  savedInstanceState.putInt(CURRENT_NUMBER, mNumber);
}

```

This allows us to access this key value pair from the savedInstanceState we pass into the `onCreate()` method, which we could update to look something like this:

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
  super.onCreate(savedInstanceState);
  setContentView(R.layout.activity_counter);
  setNumberDisplay();
  if (savedInstanceState != null) {
    mNumber = savedInstanceState.getInt(CURRENT_NUMBER);
    updateView();
  }
  setIncreaseButton();
}
```

Although I'm sure we'd refactor this to avoid resetting the number variable and the display, I hope this little example shows how Android helps a lot in maintaining state when you need to rotate your phone.
