---
title: "Property Wrappers in Swift: How to simplify your codebase and add functionality"
description: "Learn how to simplify your codebase and add functionality using Property Wrappers in Swift. Explore the basics, how to use with UserDefaults, pros, and cons to make your codebase more readable and maintainable."
date: 2023-01-13
categories: ["Swift"]
tags: ["Development", "Code"]
image: "https://drive.google.com/uc?id=1D9G6ELElIJs94vg1ZWCJouNokwU5lNTo"
draft: true
---
Swift property wrappers are a feature introduced in Swift 5.1 that allow developers to attach additional behavior or functionality to properties in a reusable, composable way. They are implemented as structs that wrap a property and provide a custom getter and setter for it. Property wrappers can be used to add functionality such as thread-safety, data validation, and key-value observing to properties.

Here is an example of a simple property wrapper that adds a "last accessed" timestamp to a property:

Copy code
@propertyWrapper
struct Timestamped<Value> {
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
    @Timestamped var name: String
}
In this example, the Timestamped property wrapper struct is defined with a generic Value type, which represents the type of the property it will be wrapping. The struct has a private stored property value to store the actual wrapped value, and a private(set) computed property lastAccessed to store the timestamp of the last access to the wrapped value. The wrappedValue computed property is the key piece of a property wrapper, it is used to provide the custom getter and setter for the wrapped property.

It can be used as following:

Copy code
let user = User(name: "John")
print(user.name)  // "John"
print(user.$name.lastAccessed)  // the time when the name was accessed
Some of the Pros of property wrapper are:

They make it easy to encapsulate complex logic and behavior related to a property in a single, reusable struct.
They make it easy to add functionality such as thread-safety, data validation, and key-value observing to properties.
They allow developers to attach additional behavior or functionality to properties in a composable way.
Some of the Cons of property wrapper are:

They can make the codebase more complex and harder to understand, particularly if they are used excessively or in a way that is not immediately obvious.
They can increase the amount of boilerplate code required to implement a feature, making the codebase more verbose.
They can add a small amount of overhead to the performance of the app due to the additional indirection introduced by the property wrapper.
It's worth noting that, Property Wrapper is a powerful feature in Swift, it can be used in many ways, such as thread-safety, data validation, key-value observing, etc. But it's important to use them judiciously and in a way that makes the codebase more readable and maintainable, not less.




raul.ferrer.dev@icloud.com
Use property wrappers with userdefaults to facilitate get and set values.
Here's an example of a property wrapper that uses UserDefaults to store and retrieve a property value:

Copy code
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
In this example, the UserDefault property wrapper struct takes a key and a default value as its initializer parameters. The wrappedValue computed property provides the custom getter and setter for the wrapped property. The getter uses the object(forKey:) method of UserDefaults to retrieve the value for the specified key, and the setter uses the set(_:forKey:) method to store the value for the specified key.

It can be used as following:

Copy code
struct Settings {
    @UserDefault("username", defaultValue: "")
    var username: String
}

let settings = Settings()
settings.username = "John"
print(settings.username)  // "John"
In this example, the username property is wrapped with the UserDefault property wrapper, with the key "username" and a default value of an empty string. When the username property is set to "John", the UserDefault property wrapper's setter is called, which stores the value "John" in UserDefaults with the key "username". When the username property is accessed, the UserDefault property wrapper's getter is called, which retrieves the value "John" from UserDefaults with the key "username"

One of the main benefits of this approach is that it abstracts away the details of reading and writing to UserDefaults, making it easy to add or change the persistence layer in the future, without affecting the rest of the codebase. Additionally, this make the code more readable and maintainable.




raul.ferrer.dev@icloud.com
Explain the @ syntax 
In Swift, the @ symbol is used to indicate that what follows is an annotation, attribute, or modifier. The most common use of the @ symbol in Swift is to indicate a property wrapper.

A property wrapper is a struct that wraps a property and provides custom getter and setter for it. Property wrappers are defined using the @propertyWrapper attribute, and they can be applied to properties using the @ symbol.

Here is an example of a property wrapper being used to add a "last accessed" timestamp to a property:

Copy code
@propertyWrapper
struct Timestamped<Value> {
    // property wrapper implementation
}

struct User {
    @Timestamped var name: String
}
In this example, @propertyWrapper is an attribute that tells the compiler that the following struct is a property wrapper, and @Timestamped is used to apply the Timestamped property wrapper to the name property of the User struct.

In addition to property wrappers, the @ symbol can also be used for other annotations, attributes, and modifiers such as @objc for Objective-C interoperability, @IBAction for Interface Builder actions, @discardableResult for indicating that the result of a function can be discarded, and more.

It's worth noting that, the @ syntax is a way to attach meta-information to the code and can be used to express intent and convey information to the developer or the compiler. The actual behavior or functionality of an @ annotation or attribute depends on the context in which it is used.




raul.ferrer.dev@icloud.com
Conclusion
In conclusion, Property Wrappers are a feature in Swift 5.1 that allows developers to attach additional behavior or functionality to properties in a reusable, composable way. Property Wrappers are implemented as structs that wrap a property and provide a custom getter and setter for it. Property Wrappers can be used to add functionality such as thread-safety, data validation, and key-value observing to properties. The @ symbol is used to indicate an annotation, attribute, or modifier, and it is most commonly used to indicate a property wrapper. Property Wrappers can be used with UserDefaults to facilitate get and set values, which makes the code more readable and maintainable. The @ syntax is a way to attach meta-information to the code and can be used to express intent and convey information to the developer or the compiler.








//////


Write a post for linkedin (with hashtags) presenting the writing of this post

Just finished writing a comprehensive guide on the topic of Property Wrappers in Swift! I covered everything from the basics of property wrappers to how to use them with UserDefaults to facilitate get and set values. Property wrappers are a powerful feature in Swift that can help simplify your codebase and add functionality in a reusable and composable way. If you're interested in learning more about property wrappers, check out the full post on my blog! #Swift #PropertyWrappers #Programming #iOSDevelopment #MobileDevelopment