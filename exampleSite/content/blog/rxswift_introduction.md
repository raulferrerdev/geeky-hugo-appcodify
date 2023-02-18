---
title: "Reactive Programming with RxSwift: Exploring the Concepts of Observer and Observable"
description: "Reactive programming is becoming increasingly popular for iOS app development, and RxSwift is a powerful framework that makes it simple to incorporate reactive programming techniques into your code. In this article, we'll look at RxSwift's two main concepts, observer and observable, and how they work together to form a reactive programming model."
date: 2018-05-29
categories: ["Architecture", "Swift"]
tags: ["Development"]
type: "regular" # available types: [featured/regular]
draft: false
---

**RxSwift** is a framework that enables reactive programming in **iOS** apps. It enables developers to write code that more intuitively and efficiently responds to changes in data and events. **RxSwift** is built on top of the ReactiveX framework, which is a cross-platform **reactive programming** implementation.

## RxSwift advantages
**RxSwift** offers several advantages to developers, including:

* Using a declarative programming model to simplify asynchronous programming.
* Reducing code complexity by eliminating callbacks and boilerplate code.
* Increasing code composability by allowing developers to easily chain operations together.
* Providing a unified method of handling events and data across multiple platforms and languages.

## Two important concepts: **Observable** and **Observer**

The **Observer** and **Observable** are the two most important concepts in **RxSwift**. 

### Observables

**Observables** generate events or data, which observers receive and respond to. This is similar to the **observer pattern**, which is a popular [design pattern](https://raulferrer.dev/design_patterns_software/) in software development.

These **events** can contain any type of data, including integers, strings, and custom objects. **Observables** can be created in a variety of ways, including converting existing asynchronous APIs, creating custom observables, and transforming existing observables with operators.

### Observer
An **Observer** is a type of object that can receive and respond to observable events. **Observers** in **RxSwift** are typically closures or functions that receive an event or data and perform some type of action in response. An observable can emit various types of events, including next events, error events, and completion events.

## Let's see an example

Let's look at an example of creating an **Observable** and **Observer** with **RxSwift**. In this example, we'll make an **Observable** that emits a random number every second, as well as an **Observer** that prints that number to the console.

```swift
import RxSwift
import RxCocoa

let disposeBag = DisposeBag()

let observable = Observable<Int>.interval(.seconds(1), scheduler: MainScheduler.instance)
    .map { _ in Int.random(in: 1...100) }

observable.subscribe(onNext: { number in
    print(number)
}).disposed(by: disposeBag)
```

First of all, we import the **RxSwift** and **RxCocoa** frameworks in this code and create a *dispose bag* to manage the subscription. The *interval* operator is then used to create an **Observable** that emits a new event every second. We also employ the *map* operator to convert the emitted events into random integers ranging from 1 to 100.

The *subscribe* method is then used to subscribe to the **Observable**. This includes a closure that is called whenever the **Observable** emits a new event. In this case, the random number is printed to the console. Finally, we dispose of the *subscription* by usinf the *disposed* method.

### Code elements
Let's se now in detail those elements:

* **Observable<Int>**

In this example, an **Observable** of tyep <Int> is generated. The type of data that will be emitted by the observable is specified by the generic parameter <Int>, so the observable will output integers in this case.

* **interval**

This operator from **RxSwift** generates an **Observable** that emits a new event every predetermined amount of time (1 second in the example).

* **map**

This is yet another operator offered by **RxSwift** that enables to modify the events that an observable emits. In the example, we utilize it to generate random integers between 1 and 100 from the emitted events.

* **subscribe**

This **RxSwift** method enables to *subscribe* to an **Observable** and get the events that it emits. In the example, we pass in a closure that will be called each time a new event is emitted and *subscribe* to the **Observable**.

* **onNext**

An **Observable** can emit this kind of event, and it represents a subsequent event that contains the **Observable**'s subsequent value. The random number is printed to the console using it in the example.

* **disposeBag**

This is an object that **RxSwift** makes available and which controls **Observable** subscriptions. A subscription can be *disposed* of when it is no longer required by using the *disposed(by:)* method on the dispose bag. In the example, we manage the *subscription* to the **Observable** we generated using the dispose bag.

## Conclusion

In this post, we focused on the Observer and Observable concepts as the foundation in order to use **RxSwift** in app development. **RxSwift** offers an effective and adaptable method for managing event-based data and **asynchronous programming** in iOS apps.