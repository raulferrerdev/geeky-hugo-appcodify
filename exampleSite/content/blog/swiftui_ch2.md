---
title: "SwiftUI #2. Avoid the 10 elements limit in VStack, HStack and ZStack"
description: "VStack, HStack and ZStack have a limit on the number of elements they can contain: 10. Let's see how we can make these structures contain more than 10 elements."
date: 2022-12-18
categories: ["SwiftUI"]
tags: ["Development", "Code"]
image: "https://drive.google.com/uc?id=1SNNrUKB-X-oydqqJJl9t6oyY6vNA-kTV"
type: "regular" # available types: [featured/regular]
draft: false
---

{{< youtube 0QJTVPfRS-0 >}}

There is a limit of 10 views that can be put directly into the VStack, HStack or ZStack structures in SwiftUI. This is so because VStack, HStack, and ZStack are constructed as generic structures, and the type parameter controls the amount of views that can be directly included in them.
{{<ads1>}}

VStack, HStack, ZStack... are views that are built using ViewBuilder. You can see that there are numerous instances of the buildBlock method, each taking a different number of views as parameters, if you look at the [ViewBuilder documentation](https://developer.apple.com/documentation/swiftui/viewbuilder). The limitation you noticed is caused by the fact that the function with the most views only accepts 10 views.
{{< image src="images/posts/swiftui_cs2_1.png" alt="SwiftUI VStack limits">}}

If we add, for example, 11 Text elements in a VStack, due to this limitation  in ViewBuild, the eleventh Text element is marked with the error 'Extra argument in call'.

{{< image src="images/posts/swiftui_cs2_2.png" alt="SwiftUI VStack limits">}}

In order to introduce more than 10 views within a VStack, an HStack, we can use different strategies.


# Group

In SwiftUI, the **Group** view is a container that wraps a group of views and allows you to apply layout and styling to them as a unit. The Group view does not have any visual effect on its own; it is simply a way to group views together and apply layout and styling to them.

As long as the total number of items does not exceed 10, the **Group** view can be used to group any number of views. We can nest any number of Groups or insert them inside other structures like **VStack** or **HStack**, which does not allow us to generate more complicated designs.

```swift
struct ContentView: View {
    var body: some View {
        VStack {
            Group {
                Text("1")
                Text("2")
                Text("3")
                Text("4")
                Text("5")
                Text("6")
                Text("7")
                Text("8")
                Text("9")
                Text("10")
            }
            Group {
                Text("11")
            }
        }
        .padding()
    }
}
```

# ForEach

Another way to add more than 10 elements is to use **ForEach** (not to be confused with Swift's *forEach()* method). **ForEach** is a struct the type **View** itself, that is, of the same type as **VStack** or **Group**, which allows us to add views dynamically.
**ForEach** works by supplying you with:
* An array of items that can be uniquely identified (so that SwiftUI can easily identify them when refreshing the view).
* A function that when executed generates a view for each element of the array.
```swift
struct ContentView: View {
    var items = ["1", "2", "3", "4", "5", "6", "7", "8", "9", "10", "11"]
    var body: some View {
        VStack {
            ForEach(0..<items.count, id: \.self) { index in
                Text(items[index])
            }
        }
        .padding()
    }
}
```
In this example, *items* is an array of strings that represents the views that we want to include in the **VStack**. The **ForEach** view will iterate over the elements in the *items* array and generate a **Text** view for each one. This allows you to include an unlimited number of views in the stack, as long as they can be represented as elements in the *items* array.

SwiftUI is instructed to utilize the entire object as the identifier by using the *\.self* command. In this instance, it instructs SwiftUI that every String value in the array is distinct and suitable for identification.

In the last method, it's critical to ensure that each value in the array is distinct because, if not, **ForEach** might improperly reuse the resulting views.
{{<ads2>}}

# List
**List** is an element, also of type View, that allows us to display a large number of elements in a vertical stack. The List view is specifically designed to display a large number of items, but adding scrolling support and optimizing performance.
```swift
struct ContentView: View {
    var items = ["1", "2", "3", "4", "5", "6", "7", "8", "9", "10", "11"]
    var body: some View {
        List {
            ForEach(0..<items.count, id: \.self) { index in
                Text(items[index])
            }
        }
        .padding()
    }
}
```

In this example, *items* is an array of strings that represents the items that we want to display in the list. The *ForEach* view will iterate over the elements in the *items* array and generate a *Text* view for each one. The **List** view will automatically handle the layout and scrolling of the items, allowing you to display an unlimited number of items in the list.
