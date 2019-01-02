Object Oriented Programming is fantastic. We can use it to bind data and behaviour into one, and that gives us a lot of power. However, with great power comes great responsibility, and handling responsibility is almost never a programmer's strong point.

Objects are incredibly popular in today's programming languages, but **what distinguishes a good object from a bad one?** There's a surprisingly large number of answers to this question, but we'll break our answer up into sections. The focus of this post will be on:

* Objects.
* Data structures.
* How/why they should _never_ overlap.

Throughout the post it will be made clear how we can use encapsulation to eliminate the overlap and write better code.

## Objects

Before we dive into data structures and encapsulation, let's ask ourselves this question. **What is an object?** Many people will have their own definitions of what an object is, and many of them will differ. For example, one might say...

* An object is an instance of a class.
* An object is a construct that holds data.
* An object is a construct that holds behaviour.
* An object is a construct that combines data and behaviour.

Notice the ambiguity here. With all of these definitions floating around, it's hard to find something that _doesn't_ qualify as "an object". It's clear that there's a lot of confusion (and potential disagreement) about what an object really is (or should be).

### What is a _true_ object?

I'm not one to dictate what a *true* object is, but I can tell you what I think it should be.

> What's your definition of an object?

To me, an object is _only_ an object if it satisfies these properties:

* It is only accessible through public methods.
* It can perform actions/has behaviour.
* It exposes absolutely _nothing_ about its implementation.
* It does not provide getters or setters.
* It manipulates the "essence" of its data, not the precise internals.

Let's discuss each of these points.

#### 1. It is only accessible through public methods

This point is fairly self explanatory. By restricting access to the variables and providing a public interface, we are **decoupling** the user of our class from the implementation.

We don't want our users to be **hoking around with the innards** of a class because it can have unintended consequences. They shouldn't care about how things are done, they should only care about the end result of an operation.

We want **full control** over our objects, and we can only do that by restricting access to their internals.

> But what if you want to expose some fields for something like an `Address`?

An `Address` wouldn't be an object, it would be a _Data Structure_. You may be wondering what the hell I'm talking about, but don't get trigger happy yet. We'll discuss this idea of "Data Structures vs Objects" in the next section.

#### 2. It can perform actions/has behaviour

We want our objects to be capable of _doing_. Objects _do_ things, and more importantly, **they do what they are told**.

There is a well-known object oriented principle called the ["Tell don't ask" principle](https://martinfowler.com/bliki/TellDontAsk.html). It states that we should **tell our objects what to do**, rather than asking them for data and acting on it.

The *only* way to follow this principle, is to have objects which _act_.

#### 3. It exposes absolutely _nothing_ about its implementation

This echoes back to the first point. The user should only care about what is done, not how it's done.

By completely hiding the implementation from the user, we are free to tweak and improve the internals of our object without their code ever needing to change.

If they can't see what's inside, it's *impossible* for them to depend on it.

#### 4. It does not provide getters or setters

Providing getters and setters completely violates all of the previous points. 

Getters and setters will...

* Completely **destroy any sense of privacy** within your object.
* Allow your user to **depend on the object's implementation**.

It's important to make the distinction between data structures and objects here. Getters and setters are _never_ appropriate for objects, however they _may_ be acceptable for data structures.

#### 5. It manipulates the "essence" of its data, not its precise internals

If we don't want to reveal the internals of our objects, our methods need to be _abstract_. (Abstract as in... more general)

Imagine we have the following Vehicle object...

```java
public interface Vehicle {
    double getFuelTankCapacityInGallons();
    double getGallonsOfGasoline();
}
```

If we think about it, these are just getters and setters in disguise. They secretly expose the internals of the Vehicle object in an innocently evil way, and need to be more abstract.

Here is an improvement of the Vehicle object that hides the implementation.

```java
public interface Vehicle {
    double getPercentFuelRemaining();
}
```

This second example is considered preferable because the user is no longer tied to a specific unit of measurement of the fuel tank. 

_(Thanks to the Clean Code book for providing this example)_

## Data Structures

What is a data structure? You may think LinkedLists or HashSets are data structures, and they _are_, but they're not the kind of Data Structure we're talking about here.

In reality, **those are objects**.

### What is a _true_ data structure?

A true data structure is... a struct! It is an **arbitrary unit of visible data**.

Pascal programmers might refer to these as "Records", and C programmers might refer to them as "Structs".

#### An example

Imagine we want to model a Person, we could do this in C like so.

```C
typedef struct {
    char* name;
    int age;
} Person;
```

Or in Java...

```Java
public class Person {
    public String name = "";
    public int age = 0;
}
```

Notice how these data structures **contain no behaviour**. They aren't supposed to. If you find yourself wanting to add behaviour to the data structure, suppress it. Create an object that performs your task instead; you'll thank yourself later.

#### Is it okay to use getters and setters?

In my opinion, maybe. This is a gray area for me because there *are* benefits to using getters and setters, although I believe it's important that they are used either consistently or not at all.

> Why "maybe"?

The only reason to have getters and setters in your data structures **is to allow you to include logic at a later date**. By doing this, you are enabling yourself to make your data structures "smarter", and doing this will violate what a data structure _is_. It could lead you to creating a hybrid object/data structure.

It's your choice, but be _very_... _VERY_ careful.

#### How do we benefit from this separation?

By separating our objects and our data structures, we are **avoiding creating hybrids**. The next section will discuss how hybrids can be damaging to the flexibility of your software.

#### Doesn't this discourage polymorphism and encourage procedural code?

Yes, but you don't _have_ to use a data structure if you're looking for polymorphism.

Uncle Bob states:

> Objects hide their data behind abstractions and expose functions that operate on that data.

* **Therefore, objects encourage polymorphic code.**

> Data structures expose their data and have no meaningful functions.

* **Therefore, data structures encourage procedural code.**

The two are virtual opposites of one another. It is important to know when to use a data structure and when to use an object.

If you find yourself updating a lot of objects to satisfy new data structures, try polymorphic objects instead. Picking the right tool for the job is important, but picking both is a deadly mistake.

## Object / Data Structure Hybrids

You *never* want to end up with a construct that is half object and half data structure. When you have a hybrid, you get all the **problems associated with data structures**, and all the **problems associated with objects**.

Your hybrids will be callable throughout your system via their public methods, but they will also have publicly visible state that tempts other functions to use them as data structures. The innards of your hybrids will **become exposed**, and the users of the code will become deeply **tied to them**.

These hybrids not only make it **difficult to add new functions** to your system (because all your polymorphic objects must change), but they also make it **difficult to add new similar data structures** (because all the functions that operate on them must change).

Before you know it, you've solidified your software into a **mess** that takes a tremendous effort to fix. You need to modify your objects, but you can't because most of the system depends on their internals.

## Summary

* Objects are **smart** (they contain behaviour).
* Objects are **shy** (they reveal nothing about their internals).
* Data structures are **dumb** (they don't contain behaviour).
* Data structures are **brash** (they reveal absolutely everything).

#### Bad objects...

* Have public fields.
* Have getters/setters.

#### Bad data structures...

* Contain behaviour.

By separating your data structures from your objects, you will inevitably write code that is easier to maintain and improve. **Binding data and behaviour is powerful, but only when done properly.**
