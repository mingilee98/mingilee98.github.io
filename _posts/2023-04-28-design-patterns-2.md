---
title: "Design Patterns 2 - The Observer Pattern"
categories:
  - Object-Oriented Programming
tags:
  - Design Patterns
---

**Previous Posts:**
[Design Patterns 1 - Why do we need them? / Strategy Pattern](/_posts/2023-04-24-design-patterns-1.md)

-------

## Learning the Observer Pattern

In this post, we are going to learn one of the most commonly used, yet useful, design patterns: the **Observer Pattern**.

With this pattern, we can design one-to-many relationships with loose coupling.

## Weather-O-Rama App

As always, we are going to learn with an example. 

We are going to make a Weather Monitoring application.

Our job is to create an app that uses the WeatherData object, which is already implemented and given, to update three displays for current conditions, weather stats, and a forecast.

![](/assets/images/0428/0428-1.jpeg)

Let's take a look at the WeatherObject class that is already implemented.

![](/assets/images/0428/0428-2.png)

The first three methods provide useful information for our display and **measurementsChanged()** gets called whenever measurements have been updated.

This time, we are going to think carefully about the **Expandability** because there can be additional displays added later in the future other than the basic three displays.

The first approach that we can think of is the following.

```java
public class WeatherData {
    public void measurementsChanged() {
        // get information
        float temp = getTemperature();
        float humidity = getHumidity();
        float pressure = getPressure();

        // update display
        currentConditionsDisplay.update(temp, humidity, pressure);
        statisticsDisplay.update(temp, humidity, pressure);
        forecastDisplay.update(temp, humidity, pressure);
    }
}
```

In this design, we simply get the information and use them to update the displays. 

What might vary is the update display section of the method. We can **encapsulate what varies**.

Also, we are coding to the implementations. This design is not flexible as we cannot add anything at runtime. 

We can apply the **Observer Pattern** to make this design better.

## the Observer Pattern

#### Definition / Visualization
Let's first define the Observer Pattern before we apply that to our design.

>**The Observer Pattern** defines a one-to-many
dependency between objects so that when one
object changes state, all of its dependents are
notified and updated automatically.

We can visualize this concept.
![](/assets/images/0428/0428-3.jpeg)
When there is a change in data in the Subject Object, all the Observer Objects are notified and updated automatically.

#### Loosely Coupled Design
Applying the observer pattern will make our previous design **loosely coupled**.

When two objects are loosely coupled, they can interact, but they typically have very little knowledge of each other.

This leads us to our fourth Design Principle.

> **Design Principle 4**
> Strive for loosely coupled designs between objects that interact.

A loosely coupled design will give us a lot of flexibility.

#### Weather-O-Rama App Revisited
Now, we will apply the observer pattern to the Weather-O-Rama App.

First, we make interfaces for the Subject Objects, Observer Objects and the Displays.

```java
public interface Subject {
    public void registerObserver(Observer o);

    public void removeObserver(Observer o);

    public void notifyObservers();
}

public interface Observer {
    public void update();
}

public interface DisplayElement {
    public void display();
}
```

Now, we implement the WeatherData class and CurrentConditionsDisplay class according to the interfaces.

```java
public class WeatherData implements Subject {
    private List<Observer> observers;
    private float temperature;
    private float humidity;
    private float pressure;

    public WeatherData() {
        observers = new ArrayList<Observer>();
    }

    public void registerObserver(Observer o) {
        observers.add(o);
    }

    public void removeObserver(Observer o) {
        observers.remove(o);
    }

    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update();
        }
    }

    public void measurementsChanged() {
        notifyObservers();
    }

    public void setMeasurements(float temperature, float humidity, float pressure) {
        this.temperature = temperature;
        this.humidity = humidity;
        this.pressure = pressure;
        measurementsChanged();
    }
}
```

```java
public class CurrentConditionsDisplay implements Observer, DisplayElement {
    private float temperature;
    private float humidity;
    private WeatherData weatherData;

    public CurrentConditionsDisplay(WeatherData weatherData) {
        this.weatherData = weatherData;
        weatherData.registerObserver(this);
    }

    public void update() {
        this.temperature = weatherData.getTemperature();
        this.humidity = weatherData.getHumidity();
        display();
    }

    public void display() {
        System.out.println("Current conditions: " + temperature
                + "F degrees and " + humidity + "% humidity");
    }
}
```

With this design, let's learn about how observers are registered and notified through the code below.

```java
public class WeatherStation {
    public static void main(String[] args) {
        // when weatherData is created, it also creates
        // a list of observers, which is empty initially.
        WeatherData weatherData = new WeatherData();

        // when currentDisplay is created, it calls
        // weatherData.registerObserver(this);
        // to register this display as an observer.
        CurrentConditionsDisplay currentDisplay = new CurrentConditionsDisplay(weatherData);

        // when the measurements are updated,
        // the observers are notified by notifyObservers()
        // which calls update() for all the observer displays.
        weatherData.setMeasurements(80, 65, 30.4f);
    }
}
```

Notice that when an Observer is notified of a change, it calls getter methods on the Subject to pull the values it needs. 

We are letting the Observers retrieve the data they need rather than passing more and more data to them through the update() method.



<br>

> ##### Reference
>*"Head First Design Patterns"* by Elisabeth Freeman and Eric Freeman