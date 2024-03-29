---
title: "Design patterns in software"
description: "Design patterns are solutions that have been found to similar problems in software development. Learn about the 23 identified design patterns."
date: 2020-02-01
categories: ["Architecture", "Swift"]
tags: ["Development", "Code"]
type: "regular" # available types: [featured/regular]
draft: false
---

## Introduction to Design patterns
Normally, in software development we often encounter problems that have similar characteristics. To solve them we find different solutions, but as some of these solutions are usually better than others (from the point of view of flexibility and reusability), they end up being preferred. These solutions end up becoming **Design Patterns** in software. **Design Patterns** in software are solutions to recurring design problems that are being applied daily in the software industry.
{{<ads1>}}

**Design patterns** allow developers to have a guide when establishing the [structure of a program, and make it more flexible and reusable](https://raulferrer.dev/blog/architecture_patterns_ios/). In addition, it also allows solving problems with a methodology already tested and not doing so from scratch.
## How many design patterns in software are there?

There are 23 **design patterns**, which were described in the book [Design Patterns, Elements of Reusable Object-Oriented Software](https://www.oreilly.com/library/view/design-patterns-elements/0201633612/) by E. Gamma , R. Helm, E. Johnson and J. Vlissides. This group of authors are considered the ‘Gang of Four’ and as the gurus of the **design patterns** in software. These authors described the 23 design patterns and classified them into three groups: **structural patterns**, creational patterns and behaviour patterns.
### Creational Patterns

**Creational patterns** are those that allow us to create objects. These patterns encapsulate the procedure for creating an object and usually work through interfaces:

* **Factory Method.** It provides an interface that allows you to create objects in a superclass, but delegates its implementation and alteration of the objects in the subclasses.
* **Abstract Factory.** It provides an interface that allows generating groups of related objects, but without specifying their specific classes or their implementation.
* **Builder.** It allows to build complex objects step by step, separating the object creation from its structure. In this way we can use the same construction process to obtain different types and representations of an object.
* **Singleton.** This pattern ensures that a class has only one possible instance, which can be accessed globally.
* **Prototype.** It allows you to copy or clone an object without the need for our code to depend on its classes.

### Structural patterns

**Structural patterns** specify how objects and classes relate to each other to form more complex structures, so that they are flexible and efficient. They rely on inheritance to define interfaces and obtain new functionalities:

* **Adapter.** It is a structural pattern that allows two objects with incompatible interfaces to collaborate, through an intermediary with which they communicate and interact.
* **Bridge.** In this pattern, an abstraction is decoupled of its implementation is decoupled, so that they can evolve independently.
* **Composite.** It allows you to create objects with a tree-like structure, and then work with these structures as if they were individual objects. In this case all the elements of the structure use the same interface.
* **Decorator.** This pattern allows new functionalities to be added to an object (including this object in a wrapper that contains the new functionalities) without modifying the behaviour of objects of the same type.
* **Facade.** It provides a simplified interza to a complex structure (such as a library or a set of classes).
* **Flyweight.** It is a pattern that allows you to save RAM, since it causes many objects to share common properties in the same object, instead of maintaining these properties in each and every object.
* **Proxy.** It is an object that acts as a simplified version of the original. A proxy controls access to the original object, which allows us to perform some tasks before or after accessing that object. This pattern is usually used in internet connections, access to device files, etc. That is to say expensive processes, and allows to reduce the cost and complexity.

### Behavioural patterns

**Behavioural patterns** are the most numerous, focus on communication between objects and are responsible for managing algorithms, relationships and responsibilities between these objects:

* **Chain of Responsibility.** It allows to pass requests through a chain of handlers. Each of these handlers can process the request or pass it on to the next one. In this way the sender and the final receiver are decoupled.
* **Command.** Transform a request into an object that encapsulates the action and the information you need to execute it.
* **Interpreter.** It is a pattern that, given a language, defines a representation for its grammar and the mechanism to evaluate it.
* **Iterator.** It allows to circulate through the elements of a collection without exposing its representation (list, stack, tree…).
* **Mediator.** It restricts direct communications between objects and forces communication through a single object, which acts as a mediator.
* **Memento.** It allows saving and restoring an object to a previous state without revealing the details of its implementation.
* **Observe.** It allows establishing a subscription mechanism to notify different objects of the events that occur in the object they observe.
* **State.** It allows an object to change its behavior when its internal state changes. It seems that his class had changed.
* **Strategy.** It allows to define that, from a family of algorithms, we can select one of them at runtime to perform a certain action.
* **Template Method.** This pattern defines the skeleton of an algorithm in a superclass, but allows subclasses to redefine some methods without changing their structure.
* **Visitor.** It allows to separate the algorithms from the objects with which they operate.
{{<ads2>}}
