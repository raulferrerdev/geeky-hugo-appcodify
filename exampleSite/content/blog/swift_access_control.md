---
title: "Access Control in Swift"
description: "Access control is a powerful feature in Swift that allows us to control the visibility of our types and members while also creating a clear and well-defined interface with which other parts of your code or modules can interact. "
date: 2023-02-27
categories: ["Swift", "Architecture"]
tags: ["Development", "Code"]
image: "https://drive.google.com/uc?id=1Dx6XK_i_lyGIG7NH0m5G80b5LJJ5ySHG"
type: "regular" # available types: [featured/regular]
draft: false
---

As a developer, one of the most important principles I learned was *encapsulation*. This principle means hiding implementation details in your code and providing a clear and well-defined interface for interacting with other parts of your code or other modules.
Swift's **access control** is a powerful tool that aids in encapsulation and the creation of more secure, modular, and easy-to-maintain code.
{{<ads1>}}


# How choose access levels?

Encapsulation is the guiding principle for Swift access levels. It creates a clear separation between the public interface of their code and their implementation details. This separation can help you reduce overlap between different parts of your code, simplify maintenance and refactoring, and improve the security and reliability of your code.

At the same time, it's important to achieve a balance between encapsulation and flexibility. If you make your code too private, it may limit your utility and flexibility. On the other hand, if it becomes too public, it may expose implementation details that may change in the future, making maintenance and evolution of its code more difficult.

# How does Swift's access control work?

Swift implements access control through five distinct levels of access: **open**, **public**, **internal**, **fileprivate**, and **private**. Each of these levels provides a different level of access to different parts of the code.

## open
**Open** is the most permissive access level in Swift. Allows to *subclass* or *override* the class, method, or property outside of the module definition. This means that other modules can extend or modify the behavior of the code.

For example:

```swift
open class vehicle {
      open func start() {
         print("Starting the vehicle...")
      }
}

open class car: Vehicle {
      override func start() {
         print("Starting the car...")
      }
}
```
As we can see, in this example we have defined a *Vehicle* class with an *open start()* method. At this point, we can define a *Car* class that inherits from *Vehicle* and overrides the *start()* method with its own implementation. Because both *Vehicle* and *start()* are marked as *open*, we can subclass *Vehicle* and override *start()* from outside the module definition.

## public
The **public** keyword is similar to **open**, but in this case, it does not allow the code to be subclassed or overridden outside of the module definition. However, it allows access to the code from any module that imports the definition module.

Let's see an example:

```swift
public class calculator {
     public func add(_a:Int, _b:Int) -> Int{
        return a + b
     }
}
```

In this case, we have defined a *Calculator* class with the public method **add()**. As *Calculator* is marked as *public*, we can access it from any module that imports the module in which *Calculator* is defined. In the same way, because *add()* is marked *public*, son we can call it from any module that imports the module definition.

## internal
The **internal** keyword is the *default access level* in Swift. **Internal** allows access to the indicated code from anywhere within the module itself, but not from outside the module.

For example:
```swift
internal class UserManager {
   var currentUser: User?

   internal func login(username: String, password: String) -> Bool {
      // Implementation...
   }
}
```
In this example we have defined a class called *UserManager* that has an **internal** *login()* method and an **internal** *currentUser* property. Since the *UserManager* is marked **internal**, it can be accessed from anywhere within the module itself, but not from outside of it. For the same reason, since the *login()* method is also marked **internal**, it can only be accessed from within the module itself.

## fileprivate
Using **fileprivate** allows code marked as such to be accessible from anywhere in the same file:

```swift
class BankAccount {
   fileprivate var balance: Double = 0.0

   func deposit(_ amount: Double) {
      balance += amount
   }

   func withdraw(_ amount: Double) -> Double {
      if balance >= amount {
         balance -= amount
         return amount
      } else {
         return 0.0
      }
   }
}
```
In this example, we define a class *BankAccount* with a **fileprivate** *balance* property and two methods: *deposit(_:)* and * withdrawal(:_)*. Any code within the same source file can instantiate *BankAccount* and access its *balance* property and methods, but code in other source files or modules cannot.

## private

**Private** access is the *lowest level of access* in Swift and only allows access to members of the definition scope. This access level is typically used for implementation details that should not be exposed outside of a single function or type:

```swift
class Person {
   private var name: String

   init(name: String) {
      self.name = name
   }

   func greet() {
      print("Hello, my name is \(name).")
   }
}
```
In this example, we have defined the class *Person* with a **private** property of name and a method *greet()* that uses that property. Because the *name* property is marked **private**, only code inside the *Person* class will be able to access this property; however, any code that has access to a *Person* instance will be able to call the *greet()* method.
{{<ads2>}}

# Conclusion

**Access control** is an important feature in Swift that allows us to control the visibility of its types and members, and create a clear and well-defined interface for other parts of your code or other modules to interact with. By using **access control**, you can encapsulate implementation details, reduce coupling between different parts of your code, make your code more readable and understandable, and optimize it for performance.