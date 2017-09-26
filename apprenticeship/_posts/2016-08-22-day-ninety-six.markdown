---
layout: post
title: Day ninety-six
date: 2016-08-22
---

The Gilded Rose
-----------------

I did the Gilded Rose Kata over the past few days.  I've tried to do it many times before, but never finished it.  Here are some thoughts I had about it whilst I was doing it.

Firstly, it scared me.

This is why I never finished it before.  This is why I watched the Sandi Metz video before and tried to be as prepared as possible.  The Gilded Rose is a refactoring exercise.  It is an exercise where you take some code that someone else has written, which happens to be hard to understand and read, as well as hard to extend, and try to add a new feature to it.  In order to extend it, you need to refactor it.  As someone who very much likes the idea of clean code, readable, elegent, simple code, I was really nervous that I would get it all wrong and make it worse and everyone would find out that I will never write clean code and I can't actually write any code at all and this whole time I've just been pretending.  These are the kinds of thoughts I have when I try to do something that I want to be good at.

I have now finished the kata and this has not happened, which is good.  It wasn't perfect, which is to be expected.  Starting was hard, but I just tried to take it slowly and go through step by step until I had something I was comfortable with.  As I went through I tried to identify the problems in the code based on what I've learnt.

My first step was write all the tests I needed.  The code worked for all the current items, so I just needed to write tests to cover all the behaviour the code initially had.  I decided I would worry about adding new behaviour when it was easy to do so.

Once my tests were written and I was confident that they would let me know if and when I broke something, I set about trying to make the code intelligible.  I had to have a vague idea of what the code was doing in order to write my tests, but I mainly used the requirements to do that, because the initial method was just too complicated for me to understand.

So the intial code looks like this:

```java
public class GildedRose {
  Item[] items;

  public GildedRose(Item[] items) {
    this.items = items;
  }

  public void updateQuality() {
    for (int i = 0; i < items.length; i++) {
      if (!items[i].name.equals("Aged Brie")
          && !items[i].name.equals("Backstage passes to a TAFKAL80ETC concert")) {
        if (items[i].quality > 0) {
          if (!items[i].name.equals("Sulfuras, Hand of Ragnaros")) {
            items[i].quality = items[i].quality - 1;
          }
        }
      } else {
        if (items[i].quality < 50) {
          items[i].quality = items[i].quality + 1;

          if (items[i].name.equals("Backstage passes to a TAFKAL80ETC concert")) {
            if (items[i].sellIn < 11) {
              if (items[i].quality < 50) {
                items[i].quality = items[i].quality + 1;
              }
            }

            if (items[i].sellIn < 6) {
              if (items[i].quality < 50) {
                items[i].quality = items[i].quality + 1;
              }
            }
          }
        }
      }

      if (!items[i].name.equals("Sulfuras, Hand of Ragnaros")) {
        items[i].sellIn = items[i].sellIn - 1;
      }

      if (items[i].sellIn < 0) {
        if (!items[i].name.equals("Aged Brie")) {
          if (!items[i].name.equals("Backstage passes to a TAFKAL80ETC concert")) {
            if (items[i].quality > 0) {
              if (!items[i].name.equals("Sulfuras, Hand of Ragnaros")) {
                items[i].quality = items[i].quality - 1;
              }
            }
          } else {
            items[i].quality = items[i].quality - items[i].quality;
          }
        } else {
          if (items[i].quality < 50) {
            items[i].quality = items[i].quality + 1;
          }
        }
      }
    }
  }
}
```

This doesn't make much sense to me, but now I had around 16 tests telling me what should happen for each item.  I wanted to use my tests as much as possible to force me to write the simplest code I could, so I followed Sandi Metz's advice and tried to ignore the giant method, whilst only dealing with a few failing tests at a time.  The easiest way to do this was to take an item, I started with the Brie, work only on what should happen to that Brie, and then return afterwards so that I was only doing one thing at a time.  So I had this:

```java
public void updateQuality() {
  for (Item item : items) {
    if (item.name.equals("Aged Brie")) {
      updateBrie(item);
      return;
    }
  }
}
```

With the rest of the method continuing afterwards.  This meant I was able to have only 4 or 5 tests failing at once, and could test drive my code to make them pass by simply running one at a time within the `updateBrie()` method.  I did the same for each different item until I could delete all the nested conditionals and my Gilded Rose looked like this:

```java
public void updateQuality() {
  for (Item item : items) {
    if (item.name.equals("Aged Brie")) {
      updateBrie(item);
    } else if (item.name.equals("Sulfuras, Hand of Ragnaros")) {
      updateHand();
    } else if (item.name.equals("Backstage passes to a TAFKAL80ETC concert")) {
      updatePasses(item);
    } else {
      updateItem(item);
    }
  }
}

private void updateItem(Item item) {
  item.sellIn--;
  item.quality--;
  if (item.sellIn < 1) { item.quality--; }
  if (item.quality < 0) { item.quality = 0; }
}

private void updatePasses(Item item) {
  item.sellIn--;
  item.quality++;
  if (item.sellIn < 11) { item.quality++; }
  if (item.sellIn < 6) { item.quality++; }
  if (item.sellIn < 1) { item.quality = 0; }
  if (item.quality > 50) { item.quality = 50; }
}

private void updateBrie(Item item) {
  item.sellIn--;
  item.quality++;
  if (item.sellIn < 1) { item.quality++; }
  if (item.quality > 50) { item.quality = 50; }
}

private void updateHand() {
}
```

And here was where I got stuck.  I could see some ways to go forward, but I wasn't really sure which way would be best.  In the Sandi Metz video, she said something that I found really interesting.  In the code above, there is obvious duplication.  Metz says that beginners are told to look for duplication and try and DRY out their code because it's simple.  Duplication requires very little coding experience to spot, and requires only a little experience to solve, but removing duplication too early can lead to the wrong abstraction.  Because I didn't have a clear idea of how I wanted this to look, I decided I would live with the duplication a little longer and see where it took me.

My initial instinct was to change the Item class.  What I wanted to be able to do was write this:

```java
for (Item item : items) {
  item.update();
}
```

But the kata specifies that the Item class should not be changed.  This forced me to look elsewhere, and forced me to add another level of abstraction.  I had a few methods that I could extract, so that's what I did.  I couldn't quite decide what to call these classes, but I went with Rules, since this abstraction dictated what should happen to an Item.  So I made a BrieRule, a BackstagePasses Rule and carried on until I was able to implement a Caonjured Rule to satisfy the new behaviour.  The Rules classes implemented the Rules interface, which looked like this:

```java
public interface Rules {

  void update(Item item);

}
```

So each Rule was responsible for updating the item given to it.  My GildedRose then looked like this:

```java
public void updateQuality() {
  for (Item item : items) {
    if (item.name.equals("Aged Brie")) {
      brieRule.update(item);
    } else if (item.name.equals("Sulfuras, Hand of Ragnaros")) {
      return;
    } else if (item.name.equals("Backstage passes to a TAFKAL80ETC concert")) {
      backstagePassesRule.update(item);
    } else {
      normalRule.update(item);
    }
  }
}
```

So the responsibility for updating the items were now within the Rules, but I still had this if statement.  I was looking at it, and knowing I didn't like it, but I took a moment to try to figure out why exactly I didn't like it.  An if statement is not inherently bad code.  It is useful and often needed.  What's wrong with this one then?  The answer was OCP.  If another item came along I would have to edit the GildedRose class to accomodate.  I had my Conjured Rule, but I would have to add another branch to the above statement to use it.  I still had my duplication between the Rules, but thought that could wait. 

To remove the if statement, I moved the responsibility of applying the Rules to the rules themselves, adding a method to the interface:

```java
public interface Rules {

  void update(Item item);

  boolean appliesTo(Item item);
}
```

So my GildedRose method became:

```java
public class GildedRose {

  private final Rules[] rules;
  public final Item[] items;

  public GildedRose(Item[] items, Rules[] rules) {
    this.rules = rules;
    this.items = items;
  }

  public void updateQuality() {
    for (Item item : items) {
      updateItem(item);
    }
  }

  private void updateItem(Item item) {
    for (Rules rule : rules) {
      applyRule(item, rule);
    }
  }

  private void applyRule(Item item, Rules rule) {
    if (rule.appliesTo(item)) {
      rule.update(item);
    }
  }
}
```

So now the GildedRose does not have to know anything about the Items it has, and doesn't care how to update them.  Another rule can be added easily by adding it to the array that is passed to the constructor.  At this point I was happy with the GildedRose class, it was the Rules I was concerned with.  My first concern was the reliance on the magic strings that let the rules know which Item was which.  I refactored this into an Enum with a title field that held the name.

Then I looked to the duplication.  There seemed to be three main groups of actions:

1. Items that decreased in quality with age
2. Items that increased in quality with age
3. Common rules shared between all items

Since I had my Rules abstraction, I worked on grouping the actions required for these types of items into different rules.  So the update method for an Item that increased in quality was `item.quality++;` and the opposite for an item that decreased and the common rules decreased the item sellIn date and ensured that the quality never went below or above the minimum and maximum qualities. There were some special cases that extended that behaviour, and I simply added seperate rules for those cases that only applied to those Items.  Since I already had my enum for the types of Item, I could use that to also register which items belonged to which group, and so determine which rules should be applied to which Items.

This was fine, other than the fact that in order for my tests to pass, the CommonRule had to be the last to applied to an item.  This is because the CommonRule simply checks whether the quality is valid, and if not, resets it to a valid number.  It does not stop the updates from happening, because I did not want to have the check in each update method, but unless the CommonRule is called last, then the quality could go out of the valid range.

It took me a while to figure out how to change this, but eventually I made my CommonRules class abstract and all the other rules extended it.  This made sense, because in essence, all the rules were just extensions of the CommonRule.  Each rule would do its specific update, and then call `super.update(item)` so that the CommonRule was applied, removing the importance of order of the rules.  

And then I stopped.  I feel like the Gilded Rose is something you could spend hours on, and now that I've done it once I would love to do it again.  I did it one way, but I'm sure there are hundreds of other ways to do it, more constraints you can add.  I really liked that I couldn't edit the Item class because it forced me to implement another abstraction which, I think, made it easier to remove duplication.  

So, that was my approach to the Gilded Rose.  I'm sure I'll do it again, and I'm not scared of it anymore, and I really enjoyed doing it.
