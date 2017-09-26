---
layout: post
title: Day one-hundred and eleven
date: 2016-09-13
---

Custom Views in Android
---------------------

In my previous version of Android Tic Tac Toe I had a problem that I wasn't sure how to get around, and it was that in order to play a mark on the board, I needed a number.  In my console and web version, the index of the button or cell chosen was easy to get hold of.  In Android, it wasn't so simple.  In my previous version I was finding each individual button by its id and storing it in an array so that I could use the array index to make a mark on the board.  As I started again, I thought there had to be a better way.  Why could I not store integers as attributes to the buttons I was using?

So, I found a way.  I'm not sure if it is the best way, but it was interesting to try it.  I made a custom button.

Custom Buttons
------------------------

In theory this seemed pretty simple.  I could just extend the Button class, add an attribute and a getter, and be able to access that in the method that needed to communicate with the game.  It did not end up being so simple.  The main concern was that I wanted to be able to set the index in the layout file rather than programmatically.  Android does support programmatic layouts, but it doesn't recommend them.  It is much more idiomatic to use the static layout files and the benefits they bring.  So how could I pass in the index without using the constructor?

Here's how I went about it.

First I made a CellButton class that extended Button.  This was the simple part.  To begin with, my CellButton looked like this:

```java
public class CellButton extends Button {
  
  private int mIndex;

  public CellButton(Context context) {
    super(context);
  }

  public int getIndex() {
    return mIndex;
  }
}
```

Then I had to move to adding a custom attribute.  In res/values I creates an attr.xml file.  I knew that you could create custom styles in this manner, so it wasn't too far of a stretch to think that I could add an attribute as well.  attr.xml looked like this:

```java
<resources>
  <declare-styleable name="CellButton">
    <attr name="index" format="integer"/>
  </declare-styleable>
</resources>
```

The declare-styleable tag just seems to declare that you want a custom attribute for the class mentioned.  Within that tag, I give the attribute a name and tell android what format the attribute will take.  In my layout file, I can then do the following:

```java
<mollie.tictactoe.ui.CellButton
  android:id="@+id/top_left"
  style="@style/CellButton"
  custom:index="0"/>
```

To allow the custom namespace, I had to import a new namespace to my layout file: 
`xmlns:custom="http://schemas.android.com/apk/res-auto"`

After that, all I needed to do was access that attribute in my CellButton class.  This was the hardest part.  I had to use an AttributeSet, and I didn't, and still don't, quite know what else is included in this Set, but my CellButton class ended up looking like this:

```java
public class CellButton extends Button {
  
  private int mIndex;
  
  public CellButton(Context context, AttributeSet attrs) {
    super(context, attrs);
    mIndex = attrs.getAttributeIntValue(3, 0);
  }
  
  public int getIndex() {
    return mIndex;
  }
}
```
The attribute that I want happens to be at index 3, which is where the magic 3 comes from, and 0 is the default value I pass in if the index is not found.

This works, but my issue with it is that it does not seem very sturdy.  I don't know why the index attribute is at position 3, and because of this:

1.  It is very hard to test
2.  I don't know what circumstances would cause it to change

I will look into this, and have created an AttributeSetFake for my tests, but the fragility of this method worries me.  What I like about it, apart from the fact that I learnt something new doing it, is that now my BoardActivity doesn't need to keep track of all the buttons. In fact, the BoardActivity doesn't care about the buttons at all.  I like that.  I'm still not sure I will stick with this method, but I enjoyed trying it out.
