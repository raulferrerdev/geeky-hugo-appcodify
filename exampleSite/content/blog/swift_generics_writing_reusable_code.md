---
title: "Swift Generics: Writing Flexible and Reusable Code"
description: "Generics are an essential tool that allow you to write code that can be adapted to work with any type, making it more flexible and reusable. In this guide, you'll learn everything you need to know about generics in Swift, including how to use generic functions, types, and constraints."
date: 2023-01-10
categories: ["Swift"]
tags: ["Development", "Code"]
image: "https://drive.google.com/uc?id=1-55gtLjg1VB7hNHtaP9MrxDOaBy0UbvU"
draft: true
---

**Generics** are a means to write code in Swift so that it is not type-specific and may be used with any type. This enables you to create **reusable**, **adaptable** code that may be applied in a variety of situations.

## Writing generic functions
A simple generic function that switches the values of two variables is shown here as an example:
```swift
func swapTwoValues<T>(_ a: inout T, _ b: inout T) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

The generic type **T**, which can be used to represent any type inside the function, is defined using the **<T>** syntax. If both arguments are of the same type, the function can then be called with any kind of arguments:

```swift
var x = 5
var y = 10
swapTwoValues(&x, &y)
// x is now 10, and y is now 5

var a = "hello"
var b = "world"
swapTwoValues(&a, &b)
// a is now "world", and b is now "hello"
```
## Writing generic types
**Generic** types, like structs and classes, can be made in addition to generic functions. Here is an illustration of a generic *Stack* type that stores its values in an array:

```swift
struct Stack<Element> {
    var items = [Element]()
    
    mutating func push(_ item: Element) {
        items.append(item)
    }
    
    mutating func pop() -> Element {
        return items.removeLast()
    }
}
```

When you create a new instance of this *Stack* type, you must define the type of the elements it will store in order to use it:

```swift
var stackOfInts = Stack<Int>()
stackOfInts.push(1)
stackOfInts.push(2)
let poppedInt = stackOfInts.pop()
// poppedInt is 2

var stackOfStrings = Stack<String>()
stackOfStrings.push("hello")
stackOfStrings.push("world")
let poppedString = stackOfStrings.pop()
// poppedString is "world"
```
## How to refactor legacy code with generics

If we want to apply the capabilities of the generics in legacy code, we will have to take into account some points. For example, you can start by identifying parts of legacy code that would profit from being made more flexible or reusable before applying generics to them. The code in those areas can then be refactored to use generic types and functions.

Consider a function that takes an array of integers as input and outputs the sum of all the array's elements:

```swift
func sumArray(_ array: [Int]) -> Int {
    var result = 0
    for number in array {
        result += number
    }
    return result
}
```
To use generics, you can restructure this method as follows:

```swift
func sumArray<T: Numeric>(_ array: [T]) -> T {
    var result: T = 0
    for number in array {
        result += number
    }
    return result
}
```
Now that the function has been updated, it can be used with any type of numeric data, including *doubles*, *floats*, and *integers*.

## Some more generics complex examples

Surely you know about the **Result** type, which represents the result of an operation, which may succeed or fail. It has two generic types: *SuccessType* and *FailureType*, which indicate the type of result that will be returned if the operation is *successful* and *unsuccessful*, respectively.

```swift
enum Result<SuccessType, FailureType> {
    case success(SuccessType)
    case failure(FailureType)
}
```

Here is an illustration of how to retrieve a list of people from a database using the **Result** type:
```swift
func getUsers() -> Result<[User], Error> {
    do {
        let users = try database.fetchUsers()
        return .success(users)
    } catch {
        return .failure(error)
    }
}
```
We can set another case for **generics**. For example, we can set a *Tree* type that describes a tree structure in which each node can have any number of child nodes and has a value of generic type **T**:
```swift
class Tree<T> {
    var value: T
    var children: [Tree<T>] = []
    
    init(value: T) {
        self.value = value
    }
    
    func addChild(_ child: Tree<T>) {
        children.append(child)
    }
}
```
Here is an example of how to make a simple family tree using the *Tree* type:
```swift
let grandfather = Tree(value: "Mark")
let grandmother = Tree(value: "Pam")
let father = Tree(value: "Robert")
let mother = Tree(value: "Sue")
let son = Tree(value: "Charles")
let daughter = Tree(value: "Anne")

grandfather.addChild(father)
grandmother.addChild(father)
father.addChild(son)
father.addChild(daughter)
mother.addChild(son)
mother.addChild(daughter)
```
## Using typealias with generics

The **typealias** keyword in Swift allows you to designate an alias for a type, which comes in handy when working with generics among other things. For example:

```swift
struct Stack<Element> {
    typealias ItemType = Element
    var items = [ItemType]()
    
    mutating func push(_ item: ItemType) {
        items.append(item)
    }
    
    mutating func pop() -> ItemType {
        return items.removeLast()
    }
}
```

In this example, the **generic** type *Element* is represented by the **typealias** *ItemType*, which is a property of the *Stack* struct. This enables you to refer to the type of the items in the stack using the more evocative word *ItemType* rather than *Element*.

## Set type constaints for generics
In Swift, **constraints** can be used to provide the specifications for the **generic** types that are utilized in a function or type. These restrictions make that the **generic** type complies with certain specifications, such as adhering to a specific protocol or being a subclass of a specific class. For example:
```swift
protocol Comparable {
    func isGreaterThan(_ other: Self) -> Bool
}

func findLargest<T: Comparable>(in array: [T]) -> T {
    var largest = array.first!
    for element in array {
        if element.isGreaterThan(largest) {
            largest = element
        }
    }
    return largest
}
```
The *findLargest(in:)* function in this illustration has a **generic** type **T** that is restricted to the *Comparable* protocol. As a result, the function can only be applied to types that follow the *Comparable* standard, and it can also invoke the *isGreaterThan(_:)* method on those types.

Type **constraints** for **generic** types can also be specified using **typealiases**. For instance:
```swift
struct Queue<Element: Equatable> {
    typealias ItemType = Element
    var items = [ItemType]()
    
    mutating func enqueue(_ item: ItemType) {
        items.append(item)
    }
    
    mutating func dequeue() -> ItemType? {
        guard !items.isEmpty else { return nil }
        return items.removeFirst()
    }
}
```
The *ItemType* **typealias** in this iteration of the *Queue* struct is restricted to the Equatable protocol, therefore the *Queue* can only be used with types that comply with *Equatable*. This enables you to compare items in the queue using the == operator.

### Multiple constaints
Additionally, more than one **constraint** may be used in a single **generic** type declaration. For instance:
```swift
func display<T: CustomStringConvertible & Comparable>(item: T) {
    print(item.description)
}
```
In this example, the **generic** type T of the *display(item:)* function is restricted to the *CustomStringConvertible* and *Comparable* protocols. This indicates that only types that abide by both of these protocols can be utilized with the function.

## Conclusion
In conclusion, Swift's use of **generics** enables the development of reusable and adaptable code by enabling programmers to create functions and types that operate on any sort of data rather than just a few predefined types. The developer can have better control over the data utilized and catch mistakes at build time rather than run time thanks to the ability to impose a type **constraint** on **generics**. This enhances the readability, maintainability, and safety of the code. Swift developers may write more reliable and effective code by utilizing **generics**, making it a crucial skill to acquire when using the language.