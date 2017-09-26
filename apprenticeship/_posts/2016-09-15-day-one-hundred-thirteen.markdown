---
layout: post
title: Day one-hundred-thirteen
date: 2016-09-15
---

Testing Android
-------------

I've found testing Android hard, but I've been determined not to let the TDD cycle slip whilst I work on my Tic Tac Toe.  As I go, there have been two things I've tried to keep in the forefront of mind.

1. Abstract logic away from Android
2. Try to have as few tests as possible that require the emulator

This has led me to develop the process that I've stuck with as I work on the project, which goes something like this:

1. Write a high level feature test.  This is my one key test that uses the emulator, and sets the boundaries for the new feature.
2. Identify any logic that I will need in order to implement that feature.  This doesn't happen all at once, but if I have a place to start, I can create a Java class that will encapsulate this, and test drive it using JUnit unit tests.
3. Use test doubles to allow test driving the integration of the logic and Android.

Step one and two have been simple.  I'm using espresso for my acceptance tests which, apart from the slightly confusing syntax, is simple to use.  These tests run slowly, so I don't want to depend heavily on them because they are a pain to run, but they are useful to frame my next steps.

So, if I use the example of a Counter App, I can demonstrate this process.

Step One
---------------

```java
@RunWith(AndroidJUnit4.class)
public class CounterActivityTest {
  
  @Rule
  ActivityTestRule<CounterActivity> mActivityTestRule = new ActivityTestRule(CounterActivity.class);

  @Test
  public void numberButtonIncreasesWhenPressed() {
    onView(withId(R.id.number_button)).perform(click());
    onView(allOf(withId(R.id.number_button), withText("1")));
  }
}
```

This is my espresso test.  In order to get this to compile, I need to add a button to my layout that has the id `number_button`.
Then I can run the test and let it fail.  After that, I can begin to think of the logic I need to get it to pass.

Step Two
-------------------

```java
public class IncrementerTest {

  @Test
  public void incrementsANumberByOne() {
    int number = Incrementer.increase(0);

    assertEquals(1, number);
  }
}
```

This unit test should cover all the logic I need, so I can make it pass:

```java
public class Incrementer {
  
  public static int increase(int number) {
    return number++;
  }
}
```

And now I am confident the logic I need is testing and works as expected, I can move to step three.

Step Three
---------------

This has been the hardest step for me so far, but also one of the most important to make me feel secure that my app works as I expect.  I can test all my logic as much as I want, but I want to be able to know what happens when I start to implement it.  I'm not really sure the best way to do this, but this is the method I have been using so far.  First, I write the test.

```java
public class CounterUnitTest {

  private CounterActivity mCounterActivity = new CounterActivity();

  @Test
  public void clickingTheButtonUpdatesTheText() {
    NumberButtonMock numberButtonMock = new NumberButtonMock();
    
    mCounterActivity.clickIncrease(mButton);

    assertTrue(mButton.setTextWasCalled(1, "1"));
  }
}
```

With this test, what I am saying is that when the button is pressed, the text is changed once, with the argument "1".  Now that my test is in place, I can begin to write my mock.

```java
public class NumberButton extends Button {
  
  public NumberButton(Context context) {
    super(context);
  }

  @Override
  public void setText(String text) {
    super.setText(text);
  }
}
```

This is my base class, and I am overriding the setText method because this is the method I want to call.  Then I can create my mock:

```java
public class NumberButtonMock extends NumberButton {
  
  private int mTimesSetTextCalled;
  private String mArgument;

  public NumberButtonMock(Context context) {
    mTimesSetTextCalled = 0;
  }

  @Override
  public void setText(String argument) {
    mTimesSetTextCalled++;
    mArgument = argument;
  }

  public boolean setTextWasCalled(int times, String argument) {
    return (times == mTimesSetTextCalled && mArgument.equals(argument));
  }
}
```

And now I can focus on making it pass.  My layout file will look like this:

```
<mollie.counter.NumberButton
  android:layout_width="wrap_content"
  android:layout_height="wrap_content"
  android:onClick="clickIncrease"
  android:id="@+id/number_button"/>
```

And my activity will look like this:

```java
public class CounterActivity extends AppCompatActivity {

  private int mNumber = 0;
  
  @Override
  public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_counter);
  }

  public void clickIncrease(View view) {
    NumberButton numberButton = (NumberButton) view;
    mNumber = Incrementer.increase(mNumber);
    numberButton.setText(mNumber);
  }
}
```

Now the test passes, and if we go back to our espresso test, we can be confident that it too will pass.

I feel a little like trying to do this has been a venture into the unknown for me, and I'm really not sure if this is the best way to do it.  In fact, I'm pretty confident there's a better way, I just don't know it yet.  I'm ok with that at the moment.  I feel like just trying to solve this problem has really focused me on what I want these tests to acheive.  Plus, they are making me feel more confident that my code will behave as expected when I run the application.

So far, this approach is working well for me with one glaring exception by the name of Bundle.  As far as I can tell, a Bundle is like a HashMap.  I have one method that takes a Bundle as an argument and returns it with values placed in it, and another that takes a Bundle object with values, and converts these values into a Game Object.  To me, this seemed like one of the easiest Android elements that I could create a double for.  But no.  The Bundle class is final and cannot be extended, so I do not know how I could create a double that could be passed in to a method that was expecting a Bundle object.  I'm not sure how to get around this at the moment.
