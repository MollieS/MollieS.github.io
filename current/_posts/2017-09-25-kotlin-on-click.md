---
layout: post
title: Using On Click Listeners in Kotlin
date: 2017-09-25
tags: android kotlin
---

In Java, setting an onClickListener usually follows a familiar Android pattern, anonymous classes.

{% highlight java linenos %}
public class MainActivity extends AppCompatActivity {

  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);

    Button button = (Button) findViewById(R.id.button);
    button.setOnClickListener(new View.OnClickListener() {
      @Override
      public void onClick(View view) {
        Toast.makeText(MainActivity.this, "i've been clicked", Toast.LENGTH_SHORT).show();
      }
    });
  }
}
{% endhighlight %}

In Java 8 which can be used when jack is enabled, the anonymous class can be replaced with a lambda

{% highlight java linenos %}
public class MainActivity extends AppCompatActivity {

  @Override
  protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);

    Button button = (Button) findViewById(R.id.button);
    button.setOnClickListener(view -> Toast.makeText(MainActivity.this, "i've been clicked", Toast.LENGTH_SHORT).show());
  }
}
{% endhighlight %}

In Kotlin it is very similar.  An onClickListener can be set as follows:

{% highlight kotlin linenos %}
class MainActivity : AppCompatActivity() {

  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)

    val button = findViewById(R.id.button) as Button
    button.setOnClickListener(object: View.OnClickListener {
      override fun onClick(view: View?) {
        Toast.makeText(this@MainActivity, "i've been clicked", Toast.LENGTH_SHORT).show()
      }
    })
  }
}
{% endhighlight %}

Or can be set with a lambda:

{% highlight kotlin linenos %}
class MainActivity : AppCompatActivity() {

  override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)

    val button = findViewById(R.id.button) as Button
    button.setOnClickListener { Toast.makeText(this@MainActivity, "i've been clicked", Toast.LENGTH_SHORT).show() }
  }
}
{% endhighlight %}

And we can also use a lambda with an argument:

{% highlight kotlin linenos %}
val button = findViewById(R.id.button) as Button
button.setOnClickListener {view -> doSomething(view)}
{% endhighlight %}

And that's pretty much all there is to it for setting an onClickListener in Kotlin
