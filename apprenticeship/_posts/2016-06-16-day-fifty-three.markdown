---
layout: post
title: Day fifty-three
date: 2016-06-16 18:40:30 +1000
---

# Test Clarity
---------

I'm redefining what it means, for me, to blog every day.  I have written a post every day this week.  I have set aside the time to write one, and I have spent that time writing.  I have not, however, posted daily.  I usually write my blog posts in the evening, when my brain is often a little tired, so often I will write a draft and leave it till the next morning to read over.  Sometimes, I don't get the time I need to look over my posts, check for typos or tidy up as much as I would like, so I think I'm going to continue to try and write a blog post every day, but I may post them in a bundle at some point during the week.  This also gives me the space to pick up a blog post that requires more research than I initially thought, and post it when it's ready.  So, although I may not post on a daily basis, I would still like to try and write every day.

Having said that, I can get on with writing about my day. 

When you're learning a lot consistently, I find it can be hard to really get a gauge on how much your learning.  Today I had a real insight on somethings I've learn during the past couple of weeks.  One of the great things about pairing is not just what you learn together from puzzling out the problem at hand, but also what you can learn from your pair.

This afternoon I was working on my own for the first time in a while, and when it came to writing my tests, I began to write them as I always had, but in the middle, I thought about something Nick had done earlier in the week.  In each of our tests he had made the set up of the test clear, made the action we were testing obvious and that made the expected values seem, well, expected.  So I used the same approach, and it made my tests so much clearer.  By reading the body of the test you could see exacly what was being tested, what values were expected and, I think, most importantly, why it was those values that were expected.  

I was testing a class that relied heavily on a storage class.  The class I was testing was in the core package, but the implementation of the interface was in the web package, so I used a StorageFake to test. The actual implementation was reading its data from a CSV file, but for the sake of my tests I just wanted that data to be available to the StorageFake easily so that it could return it when asked. 

For example, the CSVRepository could return a list of apprentice objects that it had created from the CSV, and also modify that list. To mimic that, I would ususally have written something similar to this:

```java
public class LunchManCoreTest {
  
  private LunchManCore lunchManCore;

  @Before
  public void setUp() {
    List<Apprentice> apprentices = Arrays.asList(new Apprentice("Mollie"), new Apprentice("Nick"));
    Storage storage = new StorageFake(apprentices);
    this.lunchManCore = new LunchManCore(storage);
  }

  @Test
  public void canChangeAnApprenticeAssignedToAFridayLunch() {
    lunchManCore.assignApprentice(0, "Rabea");
    assertEquals("Rabea", lunchManCore.getCurrentSchedule().get(0).getApprentice().getName());
  }
}
```

But looking back on it, that's a pretty weird test.  Why should that be Rabea?  What's actually being returned?  What's being passed in to expect that behaviour? How is that representative of the change that the name of the test suggests?  I suppose in an example this short, it's not too hard to find why that is the expected value, but really it's simpler to have it all in the test.  So I refactored, and ended up with something similar to this:

```java
public class LunchManCoreTest {

  private LunchManCore lunchManCore;
  private StorageFake storage;

  @Before
  public void setUp() {
    this.storage = new StorageFake();
    this.lunchManCore = new LunchManCore(storage);
  }

  @Test
  public void canChangeAnApprenticeAssignedToAFridayLunch() {
    storage.setApprentices("Mollie", "Nick");
    
    lunchManCore.assignApprentice(0, "Rabea");
    List<FridayLunch> schedule = lunchManCore.getCurrentSchedule();

    assertEquals("Rabea", schedule.get(0).getApprentice.getName());
  }
}
```

And suddenly the test is so much clearer!  We can deduce from the test immediatly that at the beginning of the test, the string "Mollie" is at position 0.  We change that, we load the new version of the schedule, and we expect it to have changed.  It is also clearer what it is that we are testing, and why we expect what we do.

So that's something about writing tests that I didn't even know I'd learnt, but really like.  
