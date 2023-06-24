---
title: "Mastering Swift: How to use where clause in Swift"
description: "Learn to unleash the power of the where clause in Swift in order to define additional constraints on generic types, functions, and associated types. Discover hot to use where clause in Swift to specify requirements that must be satisfied in order for a piece of code to compile and execute properly."
date: 2023-03-10
categories: ["Swift"]
tags: ["Development", "Code"]
image: "https://drive.google.com/uc?id=1bk6IL9L-rn0ZzA5CRNpcn_3DfH6sYkLA"
draft: false
---

# What is the where clause?
In Swift, the **where** clause is used to *specify additional constraints* on generic types, functions, and associated types. That is to say, it allows us to define some requirements that must be satisfied in order for the code to compile and execute properly.
{{<ads1>}}

# Uses of the where clause
Let's see some of the most common uses of the **where** clause in Swift.

## 1. Use where clause to add constraints to generic parameters
In this first case, we are going to use **where** to add a cosntraint to a generic parameter:

```swift
func findIndex<T: Equatable>(_ array: [T], value: T) -> Int? {
    for(index, item) in array.enumerated() where item == value {
        return index
    }
    return nil
}
```
In this example, the **where** clause is used to add a constraint to the generic parameter *T*, so *T* must conform to the *Equatable* protocol.

### Without where clause
If the **where** clause weren't used, we shoulb be written this code instead:
```swift
func findIndex<T: Equatable>(_ array: [T], value: T) -> Int? {
    for (index, item) in array.enumerated() {
        if item == value {
            return index
        }
    }
    return nil
}
```
Where the *if* statement checks if the current item in the array is equal to the value we are searching for, instead of using **where** to filter the elements in the *enumerate* method.

## 2. Use where clause to add constraints to associated types
In this second example, we are using the **where** clause to add constraints to associated type in a [extension](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/generics/#Extensions-with-a-Generic-Where-Clause):
```swift
protocol Container {
    associatedtype Item
    var count: Int { get }
    subscript(i: Int) -> Item { get }
}

extension Container where Item: Equatable {
    func indexOf(item: Item) -> Int? {
        for i in 0..<count whereself[i] == item {
            return i
        }
        return nil
    }
}
```
Here, with the **where** clause we are adding a constraint to the associated type *Item*, so it must conform to the *Equatable* protocol.

> An **associated type** is a protocol placeholder name that represents a type that will be determined by a conforming type. In other words, it's a way of specifying a type requirement that the conforming type will meet.

We can also add the constraint to the [associated type itself](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/generics/#Associated-Types-with-a-Generic-Where-Clause):
```swift
protocol Container {
    associatedtype Item where Item: Equatable
    var count: Int { get }
    subscript(i: Int) -> Item { get }
}

extension Container {
    func indexOf(item: Item) -> Int? {
        for i in 0..<count where self[i] == item {
            return i
        }
        return nil
    }
}
```

### Without where clause
As in the previous case, we're set now the code without the **where** clause:

```swift
protocol Container {
    associatedtype Item
    var count: Int { get }
    subscript(i: Int) -> Item { get }
}

extension Container {
    func indexOf(item: Item) -> Int? {
        for i in 0..<count {
            if self[i] == item {
                return i
            }
        }
        return nil
    }
}
```

## 3. Use where clause to add constraints to function parameters
**where** clause can be also used to add constraints to function parameters. For example:
```swift
func isEqual<T>(_ a: T, _ b: T) -> Bool where T: Equatable {
    return a == b
}
```
In this simple example, the **where** clause is used to constrain the function parameter *T*, so it must conform to the *Equatable* protocol.

### Without where claause
In this case, modifications in order to remove the **where** clause are minor:
```swift
func isEqual<T: Equatable>(_ a: T, _ b: T) -> Bool {
    return a == b
}
```
In this case, the constraint to be *Equatable* is added by the generic *T*: <T: Equatable>
## 4. Use where clause to add additional conditions to switch statements
We can also use **where** clause to add additional conditins to macth in a *switch-case* statement:
```swift
let grade = 89
switch grade {
    case 0..<60:
        print("F")
    case 60..<70:
        print("D")
    case 70..<80:
        print("C")
    case 80..<90:
        print("B")
    case 90...100 where grade % 5 == 0:
        print("A")
    default:
        print("A-")
}
```
Int this example, we use the **where** clause to match additional conditions in the *switch-case* statement. As we can see, the *case* for an 'A' grade has an additional condition that checks if the grade is a multiple of 5.

### Without where clause
As in previous cases, we can replace the **where** clause by an *if* statement:
```swift
let grade = 89
switch grade {
case 0..<60:
    print("F")
case 60..<70:
    print("D")
case 70..<80:
    print("C")
case 80..<90:
    print("B")
case 90...100:
    if (grade % 5 == 0 && grade >= 90) {
        print("A")
    } else {
        print("A-")
    }
default:
    fatalError("Invalid grade")
}
```
{{<ads2>}}

# Conclusion
Although with the use of *if* statements we can achieve the same result than with the **where** clause, **'where'** is a more concise and readable way of adding constraints. When working with complex types and constraints, with the use of the **where** clause we can make the code more efficient, expressive, and maintainable.