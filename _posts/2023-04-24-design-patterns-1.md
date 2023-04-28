---
title: "Design Patterns 1: Why do we need them? / Strategy Pattern"
categories:
  - Object-Oriented Programming
tags:
  - Design Patterns
---

## Learning Design Patterns

We will learn about design patterns that allow us to accelerate the development process and promote software reuse. 

The book that we will use is *"Head First Design Patterns"*.


## Why do we need them?

**someone has already solved our problems.**

If we can load our brains with the design patterns and apply them, we can make maintainable and reusable code.

Let's look at an example to find out how design patterns can be helpful.

We are going to create a duck pond simulation game, SimUDuck.
Now, we have a working version of this app with the class diagram shown below.

![](/assets/images/0427/0427-1.png)

It seems like we have applied Object Oriented technique of inheritance. 

However, if we want to add a new feature, which is to make some ducks fly, we find that this program is not doing so well when it comes to maintenance. 

For example, if there is a new class, RubberDuck, that inherits from the superclass Duck, we know that RubberDucks cannot quack or fly. 
Thus, we have to override the methods. 

![](/assets/images/0427/0427-2.png)

This is not a maintainable and reusable design.

Let's see how we can do better.

## Design Principles

To fix this design, we can get help from the first design principle.

> **Design Principle 1**
> Identify the aspects of your application that vary and 
> separate them from what stays the same. 

If we take the parts that vary and encapsulate them, we can alter or extend the parts that vary without affecting those that don’t.

So what is varying in this app? 
**The behaviors**.

Therefore, we can separate them from the Duck class and create a new set of classes that represent the behavior.

But how should we make the Behavior classes?

We want to keep things flexible. 
If we can assign the behaviors to the ducks at runtime dynamically, that would be a flexible design. 

The second design principle can help us to add this flexibility. 

> **Design Principle 2**
> Program to an interface, not an implementation

If we program the behaviors to an interface, we can have a flexible design. 

For example, we can make an interface called **FlyingBehavior**, and make classes according to this interface. 

Making classes for each behavior is better than providing a specialized implementation in the subclass itself because we do not have to change the subclasses every time there is a change. 

## Better Design Example

Let's look at the class diagram to see what this interface looks like.

![](/assets/images/0427/0427-3.png)

With this design, other types of objects can reuse our fly and behaviors because these behaviors are no longer hidden away in our Duck classes!

And we can add new behaviors without modifying any of our existing behavior classes or touching any of the Duck classes that use flying behaviors.

Now, we can integrate the behaviors.  

Since we have created the FlyingBehavior class, we can add these behaviors to our Duck class.

Let's remove the fly method and replace it with **performFly()** which uses the FlyingBehavior. We will also make setters so that we can change them dynamically.


```java
public abstract class Duck {
	
	FlyingBehavior flyBehavior;
	
	
	public void setFlyBehavior(FlyingBehavior fb){
                this.flyBehavior = fb;
      }
			
	public void performFly() {
		flyBehavior.fly();
	}
	
	public abstract void display();
	
}

```

Now we can change the subclasses according to this change.

```java
public class MallardDuck extends Duck{
	
	public MallardDuck() {
		flyBehavior = new FlyWithWings();
	}
	
	@Override
	public void display() {
		System.out.println("I am a Mallard Duck");
	}

}
```

Now, we can dynamically change the flying behavior, adding flexibility to our design. 

Let's look at the updated class diagram.

![](/assets/images/0427/0427-4.png)


What we have done is using **composition** instead of inheriting the behaviors, which is related to the third design principle. 

> **Design Principle 3**
> Favor composition over inheritance

Using composition can give us a lot more flexibility because we can
-  encapsulate a family of algorithms into their own set of classes
-  change behavior at runtime


## Strategy Pattern

What we did for the duck app is called the **STRATEGY
Pattern**. 

>The Strategy Pattern defines a family of algorithms,
encapsulates each one, and makes them interchangeable.
Strategy lets the algorithm vary independently from
clients that use it.

Even though we did not formally know what this pattern is, applying Object Oriented Design Principle led us to understand and use this pattern.

We can apply this pattern to other systems when we want to make the system maintainable and flexible.

Knowing these patterns is helpful because there are some object-oriented principles that underlie the patterns, and knowing this can help us even though we can't find a pattern that matches our problem.

Thus, we will learn more patterns in the upcoming posts.
<br>

> ##### Reference
>*"Head First Design Patterns" by Elisabeth Freeman and Eric Freeman*