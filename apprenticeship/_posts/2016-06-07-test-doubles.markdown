---
layout: post
title: Day fourty-five
date: 2016-06-07 15:40:00 +1000
---

Test Doubles
-----

I was introduced to test doubles quite early on in my coding adventure.  It was one of the first weekend challenges at makers where I had to introduce a random weather generator, and I had to stub the random behaviour in order to test it.  I went on to misuse test doubles frequently to the point where sometimes my tests weren't testing anything at all.

I've moved on from this and learnt from my mistakes, but it was not until I got to 8th light that I learnt that I did not know the differences between different types of doubles.  In [The Little Mocker](https://blog.8thlight.com/uncle-bob/2014/05/14/TheLittleMocker.html) Uncle Bob points out that the word "mock" has become an umbrella term for all kinds of test doubles, but this is a misnomer.

I've been working with some different types of test doubles in my Tic Tac Toe and also previously in my Rock Paper Scissors, and I had to really think about my naming, which lead me to investigate the multitude of different stubs and doubles.

### Dummy

A dummy is a type of double that has no real functionality.  It is especially useful when a class or method requires an argument that will not be used in that particular test.

So if we have this interface:

```java
public interface Weather {

  public String forecast();

}
```

And we have this class that depends on a Weather object:

```java
public class Location {

  private Weather weather;
  private String location;

  public Location (Weather weather, String location) {
    this.weather = weather;
    this.location = location;
  }

  public double showLongitude() {
    return findLongitude(location);
  }

  public String getForecast() {
    return weather.forecast();
  }

}
```

If we wanted to test the showLongitude() method, we would not care about the weather we passed in, so we could create a dummy.

```java
public class DummyWeather implements Weather {

  public String forecast() {
    return null;
  }
}
```

```java
@Test
public void returnsLongitude() {
  Location location = new Location(new DummyWeather(), "London");
  assertEquals(0.1278, location.showLongitude());
}
```

So we never actually use the weather variable with the getLongitude() method.  If we were to try, we would get a null pointer exception which is what we want, since we should not be interacting with the weather at all.

### Stub

Suppose that we want to generate a weather report for a location, and if the weather forecast suspects that there could be dangerous weather, we want our report to issue a warning, we would have something like this:

```java
public class WeatherReport {

  public WeatherReport(LocationWeather locationWeather) {
    this.locationWeather = locationWeather;
  }

  public Warning issueWarning() {
    if (locationWeather.needsWarning()) {
      return new Warning("The weather is dangerous");
    }
  }
}
```

```java
public interface LocationWeather {
  
  public boolean needsWarning();

}
```

```java
public class Location implements LocationWeather {

  private Weather weather;
  private String location;

  public Location (Weather weather, String location) {
    this.weather = weather;
    this.location = location;
  }

  public double showLongitude() {
    return findLongitude(location);
  }

  public boolean needsWarning() {
    // logic to determine whether particular weather
    // is dangerous for particular location
  }

}
```

And suppose that we had already tested the needsWarning() method so that we are confident in its behaviour.  Now, though, we simple want to test the issueWarning() method returns what we expect.  We do not have to want to create the scenario that needs a warning again, so we can stub that behaviour:

```java
public class LocationStub implements LocationWeather {

  public boolean needsWarning() {
    return true;
  }
}
```
Which would mean our test would be as simple as: 

```java
@Test
public void issuesAWeatherWarning() {
  LocationWeather locationWeather = new Location(new DummyWeather(), "London");
  WeatherReport weatherReport = new WeatherReport(locationWeather);
  assertEquals(new Warning("The weather is dangerous"), weatherReport.issueWarning());
}
```

This means that if there were ever a bug in the checks on weather a warning would need to be issued, the above test would still work, as it doesn't rely on them.  It also saves uneccesary coupling.

### Spy

Now we want to make sure that when the location checks the weather status to determine whether it is dangerous. So we can use a spy:

```java
public class WeatherSpy {

  private String currentWeatherType = "Sunny";

  public boolean wasChecked = false;

  public String forecast() {
    wasChecked = true;
    return currentWeatherType;
  }

}
```

```java
@Test
public void checksWeather() {
  WeatherSpy weatherSpy = new WeatherSpy();
  LocationWeather locationWeather = new Location(weatherSpy, "London");
  location.needsWarning();
  assertTrue(weatherSpy.wasChecked);
}
```

We can use spies to track whether a method was called, how many times or with how many and what arguments.

### Mock

Now we can discuss what a true mock is:

```java
public class WeatherMock implements Weather {

  private String currentWeatherType = "Sunny";

  public boolean wasChecked = false;

  public String forecast() {
    wasChecked = true;
    return currentWeatherType;
  }

  public boolean verify() {
    return wasChecked;
  }
}
```

Here we have changed the assertion from the test to the Mock itself, so that the WeatherMock class knows what is being tested.  It couples the tests tightly with the mock class, but it makes it easy to build Mock objects.


### Fake

Fakes are a little different to the other kinds of test doubles discussed, and you can make them behave differently depending on what kind of data you use, because a fake will act as you would expect your production code to act, providing data that will affect how the rest of your application behaves.

```java
public class WeatherFake implements Weather {

  public String forecast() {
    return "Stormy";
  }
}
```

```java
public class Location implements LocationWeather {

  private Weather weather;
  private String location;

  public Location (Weather weather, String location) {
    this.weather = weather;
    this.location = location;
  }

  public String getForecast() {
    return "The weather for " + location + " is " + weather.forecast();
  }
}
```

```java
@Test
public void locationHasAForecast() {
  LocationWeather locationWeather = new Location(new WeatherFake(), "London");
  assertEquals("The weather for London is Stormy", locationWeather.getForecast();
}
```

So, hopefully, that's a simple overview of the differences between test doubles, and when to use each of them.
