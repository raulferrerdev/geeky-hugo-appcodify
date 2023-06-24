---
title: "Mastering Enums in Swift: A Comprehensive Guide"
description: "Learn all there is to know about enums in Swift, starting with the fundamentals and moving on to more complex ideas like associated values, raw values, and custom initializers. "
date: 2023-01-09
categories: ["Swift"]
tags: ["Development"]
image: "https://drive.google.com/uc?id=1h2M3iTBC71QQSVvdaW6RUjDaYHdxDYNQ"
draft: false
---

A useful tool in any programmer's toolbox is an **enumeration**, or **enum**. They enable type-safe interaction and the grouping of related variables. **Enums** will be discussed in this post, along with how Swift may make use of them. **Enum** fundamentals as well as some more complex ideas, like related values, raw values, and custom initializers, will be covered.
{{<ads1>}}

# What are enums?

Value types called **enums** are used to express collections of linked items. They can be defined in Swift in one of two ways: with or without related values.

## Enums without associated values

**Enums without associated values** are easier to use because they don't hold any additional data for each scenario.
Think about the following **enum**, which depicts the seven days of the week: 

```swift
enum Day {
    case monday
    case tuesday
    case wednesday
    case thursday
    case friday
    case saturday
    case sunday
}

let currentday = Day.monday
```
The seven cases of the Day **enum** in this example are monday, tuesday, wednesday, thursday, friday, saturday, and sunday. A day of the week is represented by each case.

## Enums with associated values
**Enums** with associated values enable us to store extra data alongside each case. Take the **enum** that represents temperature measurements, for instance:

```swift
enum Temperature {
    case celsius(Double)
    case fahrenheit(Double)
    case kelvin(Double)
}

let tempInCelsius = Temperature.celsius(27.0)
let tempInFahrenheit = Temperature.fahrenheit(83.0)
let tempInKelvin = Temperature.kelvin(373.0)
```

The *Temperature* **enum** in this illustration has three cases: celsius, fahrenheit, and kelvin.
Each instance has a *Double* value attached to it that corresponds to the temperature reading on that scale.

## Enums with raw values

Raw values, which are connected to each case, are another option for **enums**. Any type of value, whether *Int*, *String*, or *Double*, can be a raw value. An example of an enum containing raw values is given below:

```swift
enum Planet: Int {
    case mercury = 1
    case venus
    case earth
    case mars
    case jupiter
    case saturn
    case uranus
    case neptune
    case pluto
}

let earth = Planet(rawValue: 3)
```
The *Planet* **enum** in this illustration is of type *Int* and includes nine cases. For the first few cases, the raw value is explicitly defined; for the rest cases, it is inferred.

## Enums with computed attributes and methods

The same as classes and structs, enums can have calculated properties and methods. For illustration, have a look at the updated *Temperature* **enum** from earlier:

```swift
enum Temperature {
    case celsius(Double)
    case fahrenheit(Double)
    case kelvin(Double)

    var celsiusValue: Double {
        switch self {
        case .celsius(let value):
            return value
        case .fahrenheit(let value):
            return (value - 32) * (5/9)
        case .kelvin(let value):
            return value - 273.15
        }
    }

    func compare(to otherTemperature: Temperature) -> String {
        switch (self, otherTemperature) {
        case (.celsius, .celsius): fallthrough
        case (.fahrenheit, .fahrenheit): fallthrough
        case (.kelvin, .kelvin):
            if self.celsiusValue > otherTemperature.celsiusValue {
                return "Higher"
            } else if self.celsiusValue < otherTemperature.celsiusValue {
                return "Lower"
            } else {
                return "Equal"
            }
        default:
            return "Cannot compare different temperature scales"
        }
    }
}

let temp1 = Temperature.fahrenheit(83.0)
let temp2 = Temperature.celsius(27.0)

print(temp1.compare(to: temp2)) // --> "Higher"
```

In this example, the temperature in Celsius is returned by the computed property celsiusValue of the *Temperature* **enum**, and a string indicating whether one temperature value is higher, lower, or equal to another is returned by the method *compare(to:)*, which compares two temperature values.

## Enums with custom initializers

In the same way as classes and structs do, enums can likewise have custom initializers. This can be helpful for offering many methods of initializing an enum value. For illustration, have a look at the updated *Temperature* **enum** from earlier:

```swift
enum Temperature {
    case celsius(Double)
    case fahrenheit(Double)
    case kelvin(Double)

    init(value: Double, scale: String) {
        switch scale.lowercased() {
        case "celsius":
            self = .celsius(value)
        case "fahrenheit":
            self = .fahrenheit(value)
        case "kelvin":
            self = .kelvin(value)
        default:
            self = .celsius(value)
        }
    }
}

let temp1 = Temperature(value: 77.0, scale: "Fahrenheit")
let temp2 = Temperature(value: 25.0, scale: "Celsius")
```

This example uses a custom initializer for the *Temperature* **enum**, which creates a new temperature value based on the scale and receives a value and a scale (as a string). The temperature is initialized as Celsius if the scale is not recognized.

## Enume cases as function parameters

Like any other value, **enum** cases can be utilized as function parameters. Consider the function below, which takes a *Day* **enum** case as a parameter:

```swift
func getActivities(for day: Day) -> [String] {
    switch day {
    case .monday:
        return ["go to soccer"]
    case .tuesday:
        return ["go to library"]
    case .wednesday:
        return ["visit friends"]
    case .thursday:
        return ["go to arts class"]
    case .friday:
        return ["go to shop"]
    case .saturday:
        return ["tidy up room"]
    case .sunday:
        return ["go to baseball match"]
    }
}

let dayActivities = getActivities(for: .wednesday)
print(dayActivities) // prints "visit friends"
```

The *getActivities(for:)* function in this example takes a *Day* **enum** case as an argument and outputs an array of activities based on the day.

## Iterating through the enum cases

The *allCases* attribute of an *enum* allows us to repeatedly loop through all of its cases. Take the code that iterates through all of the *Day* **enum**'s cases, for instance:

```swift
for day in Day.allCases {
    print(day)
}

// prints:
// monday
// tuesday
// wednesday
// thursday
// friday
// saturday
// sunday
```

This can be helpful for creating a list of every case that an **enum** could have.
{{<ads2>}}

# Conclusion

For organizing related data and interacting with them in a type-safe manner, **enums** are a useful tool. They can be used as function parameters and iterated through. They can also contain associated values, raw values, computed properties and methods, and custom initializers. We have examined both the fundamentals of enums and some more complex ideas in this post. I trust you now know more about how to use **enums** in your own Swift projects.