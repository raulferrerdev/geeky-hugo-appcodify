---
title: "Understanding Weak and Unowned References in Swift Closures"
description: "In Swift, closures can create strong reference cycles that lead to memory leaks and prevent objects from being deallocated from memory. To avoid this, we can use weak and unowned references in closures to create safe and efficient code. This article explains how to use [weak self] and [unowned self] to create weak references in closures, and provides examples of when to use each one. "
date: 2023-03-03
categories: ["Swift"]
tags: ["Development", "Code"]
image: "https://drive.google.com/uc?id=1rs1UkKnANeO03Y5sVzfR35k_wgjJ_--i"
draft: false
---

# Introduction
**Automatic Reference Counting (ARC)** is a memory management feature in Swift that tracks the number of references to an object to manage its lifetime. When the number of references to an object reaches zero, the object is freed up in memory.

The mechanisms used by ARC to manage the lifetime of objects are *retain* and *release*. When we make a strong reference to an object, we 'retain' it, which increases the number of references by one. When we 'release' a reference to an object, the reference count is decremented by one. When the reference count reaches zero, ARC frees the object from memory.
{{<ads1>}}

## Retain cycles
In Swift, **retain cycles**, also known as *strong reference cycles*, occur when two or more objects have strong references to one another. **Retain cycles** can prevent objects from being released from memory, resulting in *memory leaks* and poor performance over time.

Closures can also experience **retain cycles**. A **retain cycle** is created when a closure captures a reference to an object that has a strong reference to the closure. This can happen when we, for example, capture ourselves in a closure.

To avoid this, we can use *[weak self]* and *[unowned self]* in closures to create weak references to the object that created the closure.

## [weak self]
**[weak self]** is used to create a *weak reference* to the object that created the closure. This means that if the object is deallocated from memory, the *weak reference* will automatically be set to nil. Let's see and example to better understand:

```swift
class MyViewController: UIViewController {
    var buttonTapHandler: (() -> Void)?
    
    override func viewDidLoad() {
        super.viewDidLoad()
        setupButton()
    }
    
    func setupButton() {
        let button = UIButton()
        button.addTarget(self, action: #selector(buttonTapped), for: .touchUpInside)
        view.addSubview(button)
    }
    
    @objc func buttonTapped() {
        buttonTapHandler?()
    }
}
```
Initially, we've a UIViewController that has a closure (*buttonTapHandler*) to handle button actions. The view controller creates and adds a button to the view, and, when the button is tapped, *buttonTapHandler* is called.

But, because the closure has a strong reference to the view controller and the view controller has a strong reference to the closure, we've created a **retain cycle**:
```swift
buttonTapHandler = {
    self.doSomething()
}
```
To avoid this **retain cycle** we can use a weak reference to 'self':
```swift
buttonTapHandler = { [weak self] in
    self?.doSomething()
}
```
We create a *weak reference* to the view controller in the closure by using *[weak self]*. This means that if the view controller is deallocated, the closure will automatically have a nil reference to 'self', preventing the **retain cycle**.

## [unowned self]

**[unowned self]** is used to create an *unowned reference* to the object that created the closure. An *unowned reference* is similar to a *weak reference*, but, in this case, it assumes that the object will always exist for the entire lifetime of the closure (and also, this reference is not optional).
What this means is, if the object is deallocated from memory before the closure is executed, a runtime error will arise. Let's see an example of using **[unowned self]** in a closure:

```swift
class AnotherClass {
    var closure: (() -> Void)?
    
    func performAction() {
        DispatchQueue.main.async { [unowned self] in
        self.closure?()
        }
    }
}

let anotherInstance = AnotherClass()

// The closure will be executed asynchronously on the main thread
anotherInstance.performAction()
```

In this case, we're using **[unowned self]** to make a reference to 'self' that isn't owned in the closure, and *DispatchQueue.main.async* is then used to execute the closure asynchronously on the main thread. If *anotherInstance* is deallocated from memory before the closure is executed, a runtime error will arise. For this reason, we should only employ **[unowned self]** when we are certain that the object will exist through the closure.

## When to use [weak self] and when [unowned self]
As a rule of thumb:
* Use **[weak self]** when we're not sure if the object will exist for the entire lifetime of the closure.
* Use **[unowned self]** when we're sure that the object will exist for the entire lifetime of the closure.

In fact, as documentation posted in [swift.org](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/automaticreferencecounting/#Weak-and-Unowned-References):

> Define a capture in a closure as an unowned reference when the closure and the instance it captures will always refer to each other, and will always be deallocated at the same time. Conversely, define a capture as a weak reference when the captured reference may become nil at some point in the future. Weak references are always of an optional type, and automatically become nil when the instance they reference is deallocated. This enables you to check for their existence within the closure’s body.
{{<ads2>}}

# Conclusion
Understanding *weak references* and *unowned references* in Swift closures is important for avoids memory leaks and other common memory management problems. It is also important to use the appropriate type of reference for each situation, depending on whether we are confident that the object will exist for the duration of the closure or not. By following these best practices, we can write more maintainable and reliable Swift code.
