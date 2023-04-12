---
title: "Mastering Swift: A comprensive guide on Lazy variables"
description: "This article explores the concept of lazy loading in Swift programming, including the benefits and drawbacks of using lazy stored properties, and best practices for implementing lazy evaluation in various contexts such as functions, collections, views, and network requests."
date: 2023-04-12
categories: ["Swift"]
tags: ["Development", "Code"]
# image: "https://drive.google.com/uc?id=1bk6IL9L-rn0ZzA5CRNpcn_3DfH6sYkLA"
draft: false
---

## What are lazy types?

**Lazy** types are used in Swift to delay the computation or initialization of a value until it is actually required. This can be useful for improving performance and reducing memory usage, particularly when initializing a value is costly or time-consuming.

## The Lazy keyword in Swift

In Swift, use the **lazy** keyword followed by the type of the value you want to create to create a **lazy** type. For example, assume you have a *MyLazyClass* class with a property called *myLazyProperty* that takes a long time to initialize. You can make a **lazy** version of this property by doing the following:

```swift
class MyLazyClass {
    lazy var myLazyProperty: String = {
        // code to initialize myLazyProperty goes here
        return "Lazy initialization complete!"
    }()
}
```
The *myLazyProperty* property is declared as a **lazy** var of type String in this example. The property's initialization code is contained within a closure, which is passed as an argument to the **lazy var** declaration. When the *myLazyProperty* property is first accessed, the closure is executed.

It is important to note that in Swift, **lazy** types are always computed properties, even if they are initialized with a constant value. This means they can't be used as stored properties and must always be declared as *var* instead of *let*.

In Swift, **lazy** types can also be used with *functions* and *methods*. In this case, rather than being defined, the function or method is only executed when it is called:

```swift
func myLazyFunction() -> String {
    print("This function is lazy!")
    return "Lazy function executed!"
}

lazy var lazyResult = myLazyFunction()

print("This code is not lazy!")
print(lazyResult)
```
The *myLazyFunction* function is defined in this example to print a message and return a string. The *lazyResult* variable is declared as a **lazy** type and is set to the outcome of calling *myLazyFunction()*. The *print* statement follows the variable declaration and is not **lazy**; it is executed immediately. When *lazyResult* is printed, *myLazyFunction()* is finally called and the result is returned.

## Examples of lazy application

### Cache mechanism

Implementing a caching mechanism is a use case for **lazy** types in Swift. A **lazy var** can be used to cache the result of a computation the first time it is performed and then return the cached value for subsequent calls. This can be useful for expensive computations that are called frequently because it improves performance significantly:

```swift
class ExpensiveComputation {
    lazy var cachedResult: Int = {
        let result = performExpensiveComputation()
        return result
    }()

    func getResult() -> Int {
        return cachedResult
    }

    private func performExpensiveComputation() -> Int {
        // Code to perform expensive computation goes here
        return 100
    }
}

let computation = ExpensiveComputation()
print(computation.getResult()) // This will perform the expensive computation and cache the result
print(computation.getResult()) // This will return the cached result without performing the expensive computation again
```
In this example, the *ExpensiveComputation* class defines a *cachedResult* property, which is declared as a **lazy var**. This property is initialized with a closure that performs the expensive computation and caches the result the first time the property is accessed. Then, the subsequent *getResult()* calls simply return the cached result.

### Properties initiaization
Another application for **lazy** types in Swift is to initialize class or struct properties that rely on other properties. Use a **lazy var** to ensure that all dependent properties are initialized before the dependent property is initialized. This can assist you in avoiding circular dependencies and better organizing your code.
```swift
struct Point {
    let x: Int
    let y: Int

    lazy var distanceFromOrigin: Double = {
        let distance = sqrt(Double(x * x + y * y))
        return distance
    }()

    init(x: Int, y: Int) {
        self.x = x
        self.y = y
    }
}

let point = Point(x: 3, y: 4)
print(point.distanceFromOrigin) // This will initialize and return the distanceFromOrigin property
```
In this example, *Point* defines a *distanceFromOrigin* property which is declared as a **lazy var**. The property is initialized with a closure that computes the distance from the origin by using the struct's *x* and *y* properties. Because the *distanceFromOrigin* property is dependent on the *x* and *y* properties, it cannot be set until those properties are set. We can ensure that all dependencies are initialized before the *distanceFromOrigin* property is initialized by using a **lazy var**.


### Lazy views
Using UIView, lazy initialization can be used to postpone the building of the view hierarchy until the view is actually shown on the screen. By minimizing the amount of work required before the view is displayed, this can enhance performance. For example:

```swift
import UIKit

class MyViewController: UIViewController {
    
    // Define a lazy variable to store the label view
    lazy var lazyLabel: UILabel = {
        let label = UILabel()
        label.text = "Hello, world!"
        label.textColor = .black
        label.textAlignment = .center
        label.font = UIFont.systemFont(ofSize: 24)
        return label
    }()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        print("Starting viewDidLoad...")
        
        // Do some work that doesn't require the label view
        for i in 0..<10 {
            print("Working on iteration \(i)")
        }
        
        // Add the label view to the view hierarchy
        view.addSubview(lazyLabel)
        lazyLabel.translatesAutoresizingMaskIntoConstraints = false
        NSLayoutConstraint.activate([
            lazyLabel.centerXAnchor.constraint(equalTo: view.centerXAnchor),
            lazyLabel.centerYAnchor.constraint(equalTo: view.centerYAnchor)
        ])
    }
}

let myViewController = MyViewController()
```
In this example, we have a *MyViewController* view controller with a **lazy var** called *lazyLabel*, which is a UILabel object that is created with a closure that specifies the text, font, color, and alignment of the label.

As ou can see, we do some work that does not require the label view in *viewDidLoad()*, such as a loop that prints out some messages. The label view is then added to the view hierarchy by making it a subview of the view controller's view and enabling constraints to center it in the view.

When we run *MyViewController*, we see that the message *"Starting viewDidLoad..."* is printed first, then the messages from the loop, and finally the label is added to the view hierarchy. This demonstrates the advantages of **lazy** initialization with views, because we can postpone the creation of the view hierarchy until the view is actually displayed on the screen, which can improve performance by reducing the amount of work required before the view is displayed.

### Network requests
**Lazy** initialization can be also used with network requests to postpone request execution until it is actually required. This can be beneficial for applications that make frequent network requests because it reduces unnecessary network traffic.

```swift
import Foundation

class MyAPI {
    
    // Define a lazy variable to store the network response
    lazy var networkLazyResponse: String = {
        // Perform the network request
        guard let url = URL(string: "https://jsonplaceholder.typicode.com/posts/1") else {
            fatalError("Invalid URL")
        }
        let task = URLSession.shared.dataTask(with: url) { (data, response, error) in
            guard let data = data, error == nil else {
                fatalError("Network error: \(error?.localizedDescription ?? "Unknown error")")
            }
            guard let responseString = String(data: data, encoding: .utf8) else {
                fatalError("Couldn't convert response data to string")
            }
            self.networkResponse = responseString // Store the response in the lazy variable
        }
        task.resume()
        return "Performing network request..."
    }()
    
    func doSomethingWithNetworkResponse() {
        // Use the network response
        print(networkLazyResponse)
    }
}

let myAPI = MyAPI()
myAPI.doSomethingWithNetworkResponse()

```
In this example, we have a *MyAPI* class with a **lazy var** called *networkLazyResponse*, which is defined as a string with the initial value *"Performing network request..."*. The network request is carried out within the closure that initializes the **lazy** var. The response to the network request is saved in the *networkLazyResponse* variable once it has been completed.

The *networkLazyResponse* variable is then used in a function called *doSomethingWithNetworkResponse()*, and, when we create an instance of MyAPI and call *doSomethingWithNetworkResponse()*, we can see that the message *"Performing network request..."* is printed first, followed by the actual network response once the network request is complete.

## Pros and cons of lazy var

### Pros

* **Improved performance.** By deferring the computation of values until they are actually required, **lazy** initialization can improve performance by reducing unnecessary computations.
* **Reduced memory usage.** By deferring value initialization until it is required, **lazy** types can reduce memory usage, particularly for values that are expensive to compute or require a large amount of memory.
* **Simplified initialization.** **Lazy** initialization can be used to simplify the initialization of objects with complex dependencies by deferring the initialization of dependent properties until they are actually required.

### Cons
* **Increased code complexity.** Because **lazy** initialization necessitates the use of closures, it can introduce new dependencies between properties and methods.
* **Thread safety.** In multi-threaded environments, **lazy** initialization can be problematic because multiple threads may attempt to access the same **lazy** property at the same time. If the property is not properly protected, this can result in race conditions and other synchronization issues.
* **Increased memory usage.** **Lazy** initialization can reduce memory usage in some cases, but it can also increase memory usage in others because it requires the use of closures to defer initialization.

## Conclusion
In Swift, **lazy** initialization is a potent technique that can enhance efficiency and simplify your code. You can eliminate extraneous computations or data loading by waiting the creation of a variable until it is truly required, which can result in speedier and more effective code. To avoid problems like unexpected behavior if the variable is accessed before it has been initialized, it's crucial to know when to utilize and when to avoid **lazy** initialization. You may utilize **lazy** initialization to create more effective, manageable code in your Swift applications by keeping these things in mind.