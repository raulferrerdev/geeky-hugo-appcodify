---
title: "Opaque Types in Swift: Write flexible and reusable code"
description: "Learn how to use opaque types to hide code from the caller and make it more flexible and reusable. This in-depth article will walk you through the fundamentals of opaque types and demonstrate how to use them in conjunction with generics to write code that is more robust and powerful. This tutorial will provide you with the knowledge and abilities you need to advance your code, whether you are an experienced developer or a beginner."
date: 2023-01-13
categories: ["Swift"]
tags: ["Development", "Code"]
image: "https://drive.google.com/uc?id=1D9G6ELElIJs94vg1ZWCJouNokwU5lNTo"
draft: false
---

# What are Opaque Types
In Swift, an **opaque type** is a type that is defined in one module (such as a framework or library) but can only be used as a specific, concrete type within that module. The actual underlying type is hidden from any other modules that import the framework or library.

Consider a framework that specifies the **opaque type** *Authenticator* as an example. A function that accepts an instance of an authenticator as an argument and returns a Boolean value indicating whether or not the authentication was successful may be defined by the framework. The framework, however, conceals the true underlying type of the authenticator. As a result, additional modules that import the framework are unable to instantiate the *Authenticator* class or make use of any of its methods or properties.

For example:

```swift
    // In FrameworkA.swift
    public protocol Authenticator {
        func authenticate() -> Bool
    }

    public struct BasicAuth: Authenticator {
        public func authenticate() -> Bool {
            // Perform basic authentication
            return true
        }
    }

    public struct TokenAuth: Authenticator {
        public func authenticate() -> Bool {
            // Perform token-based authentication
            return true
        }
    }

    public func authenticate(with authenticator: Authenticator) -> Bool {
        return authenticator.authenticate()
    }

    // In App.swift
    import FrameworkA

    let basicAuth = BasicAuth()
    let tokenAuth = TokenAuth()

    print(authenticate(with: basicAuth))    // true
    print(authenticate(with: tokenAuth))   // true
```

This example implements a protocol called *Authenticator*, which includes two conforming structs called *BasicAuth* and *TokenAuth* and a single method *authenticate() -> Bool*. The *Authenticator* protocol instance is used by the *authenticate(with:)* function. Although the *authenticate* function is specified in *FrameworkA*, the importing module is not made aware of the true underlying type of the Authenticator.

It is simpler to alter the implementation of a framework or library without affecting current code when opaque types are used to hide implementation details and prevent clients from depending on certain kinds.

# Opaque types in SwiftUI
When used as the return type for the body field of a struct that complies with the View protocol, *some View* in SwiftUI is an **opaque type**. The *some View* type enables the caller to utilize the view like any other view type while keeping the implementation of the view hidden from the caller.

> **Opaque types** are denoted with the word *some* to show that they are an implementation detail of the function or type that is using them. It is used to convey that the caller only cares that the type follows a given protocol or has a specific set of methods and is not concerned with the precise type being used.

Here is an example of how *some View* might be used in a SwiftUI view hierarchy:

```swift
    struct ContentView: View {
        var body: some View {
            VStack {
                Text("Hello, World!")
                MyCustomView()
            }
        }
    }

    struct MyCustomView: View {
        var body: some View {
            Text("This is a custom view")
        }
    }
```

The *ContentView* in this illustration is a struct that complies with the View protocol and has a body that returns a VStack with a Text view and a *MyCustomView* view within. The body of the *MyCustomView* struct, which follows the View protocol, returns a Text view.

The real underlying types of the views that ContentView and *MyCustomView* return are concealed from the caller because the body property of both views returns some View. As long as it complies with the View protocol, this enables the implementation of *MyCustomView* to be modified without affecting the code that utilizes it.

It's important to remember that *some View* can only ever be used as return types and cannot be used as variables or constants. Additionally, it can only be used inside a struct's body property that follows the View protocol.

# Returning opaque types

When a function or method returns an **opaque type**, it signifies that the caller is not made aware of the real underlying type of the returned result. Although the caller does not have access to the underlying type's implementation details, it is nevertheless able to use the returned value.

An example of a function returning an opaque type is given below:

```swift
    func getAuthenticator() -> some Authenticator {
        return TokenAuth()
    }
```

The *getAuthenticator()* function in this example returns an **opaque type** *some Authenticator*, which means that the caller is not made aware of the true type of the given value. The caller does not have access to the implementation specifics of the underlying type but can still utilize the returned value as an instance of the Authenticator protocol.

In order to make it easier to modify the implementation of a function or method without affecting current code, returning **opaque types** might be helpful in hiding implementation details and preventing clients from depending on certain kinds. This is especially helpful when building a framework or library and giving users flexibility.

# Combine opaque types and generics
In Swift, **opaque types** and **generics** are two related ideas that can be combined to provide more adaptable and reusable code.

A type that is defined in one module (such a framework or library) but may only be used as a particular, concrete type within that module is referred to as an opaque type. Any other modules that import the framework or library are unaware of the true underlying type.

**Generics**, on the other hand, let you build code that is universally applicable to any type rather than being type-specific. This is accomplished by utilizing a placeholder type, which can be any real type at runtime and is typically represented by a single letter, such as T.

Consider a framework that defines the **opaque type** *Authenticator* and a **generic** function that accepts an instance of *Authenticator* as an input as well as a value of any type, and returns a Boolean value indicating whether or not the authentication was successful. Although the actual underlying type of the *Authenticator* is concealed from the caller, any type of value can be used with the function.

```swift
    public protocol Authenticator {
        func authenticate<T>(value: T) -> Bool
    }

    public struct BasicAuth: Authenticator {
        public func authenticate<T>(value: T) -> Bool {
            // Perform basic authentication on the value
            return true
        }
    }

    public struct TokenAuth: Authenticator {
        public func authenticate<T>(value: T) -> Bool {
            // Perform token-based authentication on the value
            return true
        }
    }
```
In this instance, the framework defines the *authenticate* function, which accepts any type as a value. The importing module is not informed of the true *Authenticator*'s underlying type. By doing so, the framework can operate on any kind of value while still keeping the caller's implementation of the *Authenticator* a secret.

**Generics** and **opaque types** can be combined to produce more adaptable, reusable code that is hidden from the caller. This can be especially helpful when developing frameworks or libraries because it enables you to give users flexibility while keeping control over the implementation specifics.

# Conclusion

Finally, **opaque types** are a means to establish a type in Swift that is only utilized as a specific, concrete type within a module, while concealing the real underlying type from other modules that import the framework or library. As a result, changes to the type's implementation can be made without having an impact on the code that employs it.

When developing frameworks or libraries, **opaque types** can be very helpful since they let you provide users choice while yet keeping control over the implementation specifics. As the return type for the body field of a struct that complies with the View protocol, they are also helpful in SwiftUI.

**Opaque types** can produce more adaptable, reusable code that is hidden from the caller when used with generics. Any type can be used, and the caller is still kept in the dark about the implementation.

Overall, **opaque types** are an effective technique for writing more adaptable, reusable code that is concealed from the caller. They may be applied in a number of contexts to increase the robustness and maintainability of your code.