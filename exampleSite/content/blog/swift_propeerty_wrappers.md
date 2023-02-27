---
title: "Property Wrappers in Swift: How to simplify your codebase and add functionality"
description: "Learn how to simplify your codebase and add functionality using Property Wrappers in Swift. Explore the basics, how to use with UserDefaults, pros, and cons to make your codebase more readable and maintainable."
date: 2023-01-13
categories: ["Swift"]
tags: ["Development", "Code"]
image: "https://drive.google.com/uc?id=1D9G6ELElIJs94vg1ZWCJouNokwU5lNTo"
draft: true
---

# What are property wrappers?

**Property wrappers** are a feature introduced in Swift 5.1 that allows developers to add behaviors or functionality to properties in a reusable and composable way. These are structures that 'wrap' a property and provide a custom *getter* and *setter* for it.

Thus, for example, we can use property containers to add functionality such as: thread safety, data validation, or observation of key-value pairs of properties.

> **The @ syntax.** In Swift, the @ symbol is used to indicate that what follows is an annotation, attribute, or modifier (as in *@propertyWrapper*).

## Example of a property wrapper
For example, suppose we have a property to which we want to add a **property wrapper** that allows us to know when the property was last accessed:

```swift
@propertyWrapper
struct TimeStamp<Value> {
    private var value: Value
    private(set) var lastAccessed: Date

    var wrappedValue: Value {
        get {
            lastAccessed = Date()
            return value
        }
        set {
            value = newValue
        }
    }

    init(wrappedValue: Value) {
        self.value = wrappedValue
        self.lastAccessed = Date()
    }
}

struct User {
    @TimeStamp var name: String
}
```
In this example, the *TimeStamp* wrapper property is defined with a generic value type, which represents the type of property it will wrap. *TimeStamp* has a private stored property, *value*, that stores the actual wrapped value and a private computed property, *lastAccessed*, that will store the timestamp of the last access to the wrapped value. The *wrapValue* computed property is the core of a property wrapper, as it is used to provide the custom getter and setter for the wrapped property.

Let's se how to use this new property wrapper:

```swift
let user = User(name: "John")
print(user.name)  // "John"
print(user.$name.lastAccessed)  // the time when the name was accessed
```

## Useful applications of property wrappers
In applications, one of the most common ways to save information is using *UserDefaults*, but when using this system we find all the code to retrieve and save data. Let's see if we can simplify it by using property wrappers.

```swift
@propertyWrapper
struct UserDefault<T> {
    let key: String
    let defaultValue: T

    init(_ key: String, defaultValue: T) {
        self.key = key
        self.defaultValue = defaultValue
    }

    var wrappedValue: T {
        get {
            return UserDefaults.standard.object(forKey: key) as? T ?? defaultValue
        }
        set {
            UserDefaults.standard.set(newValue, forKey: key)
        }
    }
}
```
In this example, the wrapper structure for the *UserDefault* property takes a *key* and a *defaultValue* as its initialization parameters. The *wrapValue* computed property provides the custom getter and setter for the wrapped property:

* The getter uses the *object(forKey:)* method of *UserDefaults* to retrieve the value of the specified *key*.

* The setter uses the *set(_:forKey:)* method to store the value of the specified *key*.

```swift
struct Settings {
    @UserDefault("username", defaultValue: "")
    var username: String
}

let settings = Settings()
settings.username = "John"
print(settings.username)  // "John"
```
As we can see, the *username* property is wrapped with the UserDefault property wrapper (with the *key* 'username' and an empty string as a default value). When the *username* property is set to 'John', the *UserDefault* property wrapper's setter is called, ant it stores the value 'John' in *UserDefaults* with the key *username*. When the *username* property is accessed, the getter of the *UserDefault* property wrapper is called, which returns the value 'John'.

With this we have achieved to abstract away the details of reading and writing to *UserDefaults*, making it easy to add or change the persistence layer in the future, without affecting the rest of the codebase.

# Pros and cons of property wrappers

## Pros
* They make it simple to encapsulate complex property logic and behavior in a single and reusable struct.
* They make it simple to add thread-safety, data validation, and key-value observing to properties.
* They enable developers to add additional behavior or functionality to properties in a modular fashion.

# Cons

* They can make the codebase more complex and difficult to understand, especially if used excessively or in unexpected ways.
* They can make the codebase more verbose by increasing the amount of boilerplate code required to implement a feature.
* Because of the additional indirection introduced by the property wrapper, they can add a small amount of overhead to the app's performance.

# Conclusion
Finally, **property wrappers** are a feature in Swift 5.1 that allows developers to attach extra behavior or functionality to properties in a reusable, composable manner.
