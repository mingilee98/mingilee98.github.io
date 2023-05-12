---
title: "[Design Patterns] The Factory Pattern"
categories:
  - Object-Oriented Programming
tags:
  - Design Patterns
---

**Previous Posts:**

[[Design Patterns] Why do we need them? / The Strategy Pattern](/_posts/2023-04-24-design-patterns-1.md)

[[Design Patterns] The Observer Pattern](/_posts/2023-04-28-design-patterns-2.md)

[[Design Patterns] The Decorator Pattern](/_posts/2023-04-29-design-patterns-3.md)

-------

## Learning the Factory Pattern

In this post, we are going to learn the **Factory Pattern**.

The factory patterns can help us to create some loosely coupled designs.

## Problem with the new operator

When we are using the **new** operator, we are building something that is concrete.

However, tying your code to something concrete can make the problem less flexible.

For example, the code below has to be opened and modified whenever there is a new extension. 

```java
Duck duck;
if (picnic) {
    duck = new MallardDuck();
} else if (hunting) {
    duck = new DecoyDuck();
} else if (inBathTub) {
    duck = new RubberDuck();
}
```

Therefore, the code above is not **closed for modification**.

Factory patterns can help us to avoid this situations

## The Simple Factory

We will look at different types of factory patterns using the pizza shop example.

Let's say that we are going to have a **orderPizza()** method that looks like below.

```java
Pizza orderPizza(String type) {
    Pizza pizza;

    // concrete classes instatiated with new operator
    if (type.equals("cheese")) {
        pizza = new CheesePizza();
    } else if (type.equals("greek")) {
        pizza = new GreekPizza();
    } else if (type.equals("pepperoni")) {
        pizza = new PepperoniPizza();
    }

    pizza.prepare();
    pizza.bake();
    pizza.cut();
    pizza.box();
    return pizza;
}
```

In the design above, we have to modify the **orderPizza()** method whenever there is a change in our menu.

We can use a simple **encapsulation** to fix this problem.

That is, we can move the creating pizzas part out into another object that is only going to be concerned with creating pizzas.

```java
public class SimplePizzaFactory {
    public Pizza createPizza(String type) {
        Pizza pizza = null;
        if (type.equals("cheese")) {
            pizza = new CheesePizza();
        } else if (type.equals("pepperoni")) {
            pizza = new PepperoniPizza();
        } else if (type.equals("clam")) {
            pizza = new ClamPizza();
        } else if (type.equals("veggie")) {
            pizza = new VeggiePizza();
        }
        return pizza;
    }
}

public class PizzaStore {
    SimplePizzaFactory factory;

    public PizzaStore(SimplePizzaFactory factory) {
        this.factory = factory;
    }

    public Pizza orderPizza(String type) {
        Pizza pizza;
        pizza = factory.createPizza(type);
        pizza.prepare();
        pizza.bake();
        pizza.cut();
        pizza.box();
        return pizza;
    }
}
```

Here, we have moved the creating pizza part out into the **SimplePizzaFactory** class, and gave **PizzaSotre** a reference to that factory. 

In the **orderPizza** method of the **PizzaStore** class, we are using the factory to create the pizza.

This **Simple Factory** design is not a design pattern, but more of a commonly used idiom. 

Now, we will use two heavy-duty patterns that are both factories.

## The Factory Method Pattern

<br>

> ##### Reference
>*"Head First Design Patterns"* by Elisabeth Freeman and Eric Freeman