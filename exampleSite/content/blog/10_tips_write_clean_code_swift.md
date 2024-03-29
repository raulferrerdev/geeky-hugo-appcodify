---
title: "10 Tips for Writing Cleaner and More Readable Swift Code"
description: "Writing clean and readable Swift code is essential for any developer. Here are ten tips to help you achieve this goal, including using descriptive variable names, minimizing nesting, and using optionals and protocols."
date: 2023-04-13
categories: ["Swift"]
tags: ["Development", "Code"]
image: "https://drive.google.com/uc?id=11WoHEToCNibtBwHfeu8RpeyJJ7noiSpU"
draft: false
---

## Clean code in Swift

**Writing clear and legible code** is crucial for Swift developers. In the long run, it makes code simpler to comprehend, maintain, and modify. Here are some guidelines to help you create Swift code that is clearer and easier to read.
{{<ads1>}}

### 1. Observe naming standards

Variables, functions, and classes in Swift all have unique names. We can **improve the readability and comprehension** of our code by adhering to certain practices. Function names, for instance, ought to include a verb-noun combination that describes what the function does. All variable names should begin with a lowercase letter and be written in *camelCase*. Classes should begin with an uppercase letter and utilize *PascalCase*. Here's an example:

```swift
class StudenGrades {  // PascalCase
    func calculateAverage(numbers: [Double]) -> Double { // camelCase
        var total = 0.0
        for number in numbers {
            total += number
        }
        return total / Double(numbers.count)
    }
}
```

In this example, we have used a verb-noun pair that specifies the function's purpose to name it. The method utilizes a return statement to make its intent clear and the variable is also named using *camelCase*.



### 2. Use blank space
Our code may be easier to read and understand if we use *blank*. When dividing logical chunks of code, such as those between functions, loops, and conditional expressions, use *blank* space. For example:

```swift
func calculateAverage(numbers: [Double]) -> Double {
    
    var total = 0.0
    
    for number in numbers {
        total += number
    }
    
    return total / Double(numbers.count)
}
```

The function signature, the variable declaration, the *for-in* loop, and the return statement are all separated from one another in this example by *blank* space. This makes it simpler to quickly identify the various sections of the code.

### 3. Keep it simple
When it comes to writing clear and understandable code, simplicity is essential. Keep our reasoning simple so that it is easier to grasp what the code is doing. Break up difficult jobs into smaller, easier to handle components:

```swift
func checkGrade(score: Int) -> String {
    if score >= 90 {
        return "A"
    } else if score >= 80 {
        return "B"
    } else if score >= 70 {
        return "C"
    } else if score >= 60 {
        return "D"
    } else {
        return "F"
    }
}
```

In this example, the function calculates the grade depending on the score using a series of conditional statements. This method makes the code more readable because it is straightforward and simple to comprehend.

### 4. Comment code
The intent behind the code, the intended input and output, as well as any restrictions or known concerns, can all be explained using comments. Make your code more readable and understandable by adding comments. For example:

```swift
/// This function calculates the average of an array of numbers
/// - Parameter numbers: An array of doubles
/// - Returns: The average of the array
func calculateAverage(numbers: [Double]) -> Double {
    // Check if the array is empty
    guard !numbers.isEmpty else { fatalError("Array is empty") }
    
    // Calculate the total of the array
    var total = 0.0
    for number in numbers {
        total += number
    }
    
    // Calculate the average and return it
    return total / Double(numbers.count)
}
```

In this example, the purpose, input, and output of the function are described with comments. This improves the readability and comprehension of the code.

### 5. Use guard statements
Code can be made simpler by handling error cases up front with **guard** statements. Check for invalid input or other error conditions with **guard** statements to stop code execution. Here's an example:
```swift
func calculateAverage(numbers: [Double]) -> Double {
    
    guard !numbers.isEmpty else {
        fatalError("Array is empty")
    }
    
    var total = 0.0
    
    for number in numbers {
        guard number >= 0 else {
            fatalError("Negative numbers are not allowed")
        }
        total += number
    }
    
    return total / Double(numbers.count)
}
```

The first **guard** statement in this illustration determines whether the array is empty and halts execution if it is. The execution is halted by the second **guard** statement if any of the numbers are negative.

### 6. Apply enums
Swift's **enums** feature is a potent one that can help make code easier to read and comprehend. To create a group of connected values or cases that can be used across your code, create **enums**:

```swift
enum Color {
    case red, green, blue
}

let color = Color.red

switch color {
    case .red:
        print("This is red")
    case .green:
        print("This is green")
    case .blue:
        print("This is blue")
}
```

In this example, a set of colors is defined using an **enum**. The **enum** is then used by the **switch** statement to determine which color is being used.

### 7. Use optionals
Another potent tool in Swift that can help your code be more readable and understandable is **optionals**. In order to represent values that might or might not exist, we use **optionals**. Our code may become more adaptable and less prone to errors as a result:
```swift
var name: String? = "John"
var age: Int? = nil

if let name = name {
    print("Name is \(name)")
}

if let age = age {
    print("Age is \(age)")
} else {
    print("Age is unknown")
}
```

The name and age of a person are represented in this example using **optionals**. The first **if statement** determines whether the name is present and prints it if it is. When the age is present, the second **if statement** prints it; otherwise, a message is printed.

### 8. Use extensions
Classes, structs, and enums that already exist can have new functionality added to them using **extensions**, allowing us to keep our code organized and simple to understand. For example:

```swift
extension Double {
    func round(to places: Int) -> Double {
        let divisor = pow(10.0, Double(places))
        return (self * divisor).rounded() / divisor
    }
}

let number = 3.14159265359
print(number.round(to: 2))

```

In this case, a *round* method is added to the *Double* class using an **extension**. This makes rounding numbers to a particular number of decimal places simple.

### 9. Use protocols
Using **protocols**, we can specify a set of specifications that classes or structs must follow (sharing behavior among various object types):

```swift
protocol Drawable {
    func draw()
}

class Circle: Drawable {
    func draw() {
        print("Drawing a circle")
    }
}

class Square: Drawable {
    func draw() {
        print("Drawing a square")
    }
}

let circle = Circle()
let square = Square()

let shapes: [Drawable] = [circle, square]

for shape in shapes {
    shape.draw()
}
```

In this example, a drawable object is represented by a **protocol** called *Drawable*. The *draw* method is implemented by the *Circle* and *Square* classes, which follow the *Drawable* **protocol**. Both *Circle* and *Square* objects are present in the shapes array, and both are then drawn using the draw method.

### 10. Utilize functional programming methods
The use of functions to create modular, reusable code is emphasized in the **functional programming paradigm**. [Higher-order functions](https://raulferrer.dev/blog/swift_high_order_functions/) and closures are just two of the many features that Swift offers to make functional programming simple. Apply **functional programming** techniques to your code to make it clearer and more concise:

```swift
let numbers = [1, 2, 3, 4, 5]

let filteredNumbers = numbers.filter { $0 % 2 == 0 }
let mappedNumbers = filteredNumbers.map { $0 * 2 }
let reducedNumbers = mappedNumbers.reduce(0, +)

print(reducedNumbers)
```

The *filter* function is used to choose even numbers from an array in this example. The even numbers are then each doubled using the *map* function. The array that results is summarized using the *reduce* function. This is an illustration of how to write clear, readable code using functional programming techniques.
{{<ads2>}}

## Conclusion

Writing clear and understandable Swift code is a important component of being a good developer, to sum up. We can enhance the quality of our code and make it simpler for us and others to comprehend and maintain by paying attention to the ten suggestions provided in this article. Always we must remember to aim for consistency, modularity, and simplicity in your code.


