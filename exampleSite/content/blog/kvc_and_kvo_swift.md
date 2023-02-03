---
title: "KVC and KVO in iOS Development: Simplifying Dynamic and Reactive Code"
description: "In this post, we will delve into the world of KVC (Key-Value Coding) and KVO (Key-Value Observing) in iOS development, two powerful technologies that allow you to dynamically access and modify the properties of objects. By the end of this post, you will have a solid understanding of how to use KVC and KVO to simplify your code and make your iOS apps more dynamic and reactive."
date: 2018-05-18
categories: ["Swift"]
tags: ["Development"]
type: "regular" # available types: [featured/regular]
draft: false
---

KVC and KVO are two important concepts in iOS app development, especially when using Swift. These two frameworks enable developers to access and manipulate object properties while also observing changes to properties in real time. In this post, we'll go over KVC and KVO in great detail, explaining what they are, how they work, and how to use them effectively in your iOS app.

## KVC (Key-Value Coding)

**KVC** is an abbreviation for **Key-Value Coding**, and it's a mechanism for accessing an object's properties via string keys rather than direct access. This is especially useful if you don't know what properties an object has or if you want to access properties dynamically based on user input. We must use the *value(forKey:)* method to use **KVC**, which returns the value of the specified key.

For example, let's consider ta typical *Person* class, which has properties such as name, age, and address:

```swift
class Person: NSObject {
    var name: String
    var age: Int
    var address: String

    init(name: String, age: Int, address: String) {
        self.name = name
        self.age = age
        self.address = address
    }
}
```

It should be noted that **KVC** can only be applied to *NSObject* subclasses. This is due to the fact that **KVC** is dependent on the Objective-C runtime, which is only available to *NSObject* subclasses.

If we have an instance of the *Person* class, we can use **KVC** to access its properties as follows:

```swift
let person = Person(name: "Liz", age: 35, address: "123 5th Ave")
let name = person.value(forKey: "name") as? String
let age = person.value(forKey: "age") as? Int
```

## KVO (Key-Value Observing)

**KVO** is an acronym that stands for **Key-Value Observing**. It is a mechanism for observing changes in an object's properties in real time. When a property is observed, an alert is sent whenever the property changes. This is helpful for keeping views and other components up to date with model data.

**KVO** is activated by calling the *addObserver(:forKeyPath:options:context:)* method, which adds an **observer** for a specific key path. For example, let's continue with the *Person* class:

```swift
person.addObserver(self, forKeyPath: "name", options: [.new, .old], context: nil)
```

The **observer** will always be notified of changes to the name property together with the property's current and previous values. You must override the *observeValue(forKeyPath:of:change:context:)* method in order to handle these notifications:

```swift
override func observeValue(forKeyPath keyPath: String?, of object: Any?, change: [NSKeyValueChangeKey : Any]?, context: UnsafeMutableRawPointer?) {
    if keyPath == "name" {
        let newValue = change?[.newKey] as? String
        let oldValue = change?[.oldKey] as? String
        print("Name changed from (oldValue) to (newValue)")
    }
}
```
We must use the *removeObserver(:forKeyPath:)* function to delete the **observer** once we are done watching a property.

**KVO** only works for attributes that are declared as **dynamic**; it is important to remember this. This is so that the Objective-C runtime, which **KVO** depends on, may construct accessor methods for the observed property dynamically.