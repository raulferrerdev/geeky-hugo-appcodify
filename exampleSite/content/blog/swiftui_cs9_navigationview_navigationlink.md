---
title: "SwiftUI #9. Navigation in SwiftUI: NavigationStack"
description: "This chapter will go over how to use NavigationStack and NavigationLink, which are the two main components for managing the navigation stack and navigating between views in an app. The post describes how to use these components in detail, as well as how to customize their appearance and behavior with built-in modifiers. The guide also includes examples and explanations of how to pass data between views and control navigation programmatically."
date: 2023-01-26
categories: ["SwiftUI"]
tags: ["Development", "Code"]
image: "https://drive.google.com/uc?id=1lUjUTZnNmq3FPdNGv0nvVLnj-qIHZUrU"
type: "regular" # available types: [featured/regular]
draft: false
---

{{< youtube FAcwmuamy5I >}}

This post will go over how to use NavigationStack and NavigationLink, which are the two main components for managing the navigation stack and navigating between views in an app. 
{{<ads1>}}

## NavigationView Deprecation in iOS 16.2

About six months ago, Apple announced that the **NavigationView** will be [deprecated](https://developer.apple.com/documentation/swiftui/navigationview) in iOS 16.2. **NavigationView** was the primary method of managing an app's various levels, from the main menu to the deeper levels. It enabled developers to create complex, hierarchical apps quickly and easily. Its straightforward design, which was consistent across Apple's platforms, made it a popular choice among developers.
However, the **NavigationView** will be deprecated in iOS 16.2. As a result, developers will need to consider alternative solutions for managing the various levels of their apps. Apple has provided some guidance on the best course of action.

## NavigationLink Methods Deprecation in iOS 16.0

Although Apple has not deprecated the **NavigationLink** component (which allows us to push from a new perspective), it has deprecated some of its [methods](https://developer.apple.com/documentation/swiftui/navigationlink/init(_:isactive:destination:)-6xw7h) as of iOS 16.0. It's the case of methods like *init( :isActive:destination:)* or *init( :tag:selection:destination:)*, which allow you to check if a link is active (*isActive*) or pass data to a new window (*tag*).
## Before iOS 16.2

This is an example of how NavigationView was used before its deprecation:

```swift
struct ContentView: View {
    let items = [Item(name: "Item 1"), Item(name: "Item 2"), Item(name: "Item 3")]

    var body: some View {
        NavigationView {
            List(items) { item in
                NavigationLink(destination: DetailView(item: item)) {
                    Text(item.name)
                }
            }
            .navigationBarTitle("Home")
            .navigationBarItems(leading:
                Button(action: {
                    print("Leading button tapped")
                }) {
                    Image(systemName: "list.bullet")
                }, trailing:
                Button(action: {
                    print("Trailing button tapped")
                }) {
                    Image(systemName: "heart")
                }
            )
            .toolbar(.hidden)
            .navigationBarBackButtonHidden(true)
        }
    }
}

struct DetailView: View {
    var item: Item

    var body: some View {
        VStack {
            Text("Details for: \(item.name)")
            NavigationLink(destination: ContentView(), isActive: .constant(false), label: {
                Text("Go back to Home")
            })
        }
        .navigationBarTitle("Detail View")
    }
}

struct Item: Identifiable {
    let id = UUID()
    let name: String
}
```

## From iOS 16.2: NavigationStack

On iOS 16.2, Apple changed NavigationView by NavigationStack. According to [Apple](https://developer.apple.com/documentation/swiftui/navigationstack):

> A view that displays a root view and enables you to present additional views over the root view.

That is, it is a root view to which more views can be added on top (*push*).

Let's see an example:

```swift
struct ContentView: View {
    let items = [Item(name: "Item 1"), Item(name: "Item 2"), Item(name: "Item 3")]
    
    var body: some View {
        NavigationStack {
            List(items) { item in
                NavigationLink(destination: DetailView(item: item)) {
                    Text(item.name)
                }
            }
            .navigationTitle("Home")
        }
    }
}

struct DetailView: View {
    var item: Item
    
    var body: some View {
        VStack {
            Text("Details for: \(item.name)")
        }
        .navigationTitle("Detail View")
    }
}
```
It's the same example as the old **NavigationView** example. We only have changed **NavigationView** with **NavigationStack**, but it has the same behaviour: shows some links and, opn tap on ones of those links (**NavigationLink**), the DetailedView is pushed to the stack.

## Data-driven navigation
With the introduction of the **NavigationStack**, Apple has also introduced a *data-driven* navigation model. What does this mean, since it has added a new modifier (*navigationDestination*) that relates the destination to which you want to navigate with a specific data type.

Let's modify the previous example in order to use *navigationDestination*:

```swift
struct ContentView: View {
    let items = [Item(name: "Item 1"), Item(name: "Item 2"), Item(name: "Item 3")]
    
    var body: some View {
        NavigationStack {
            List(items) { item in
                NavigationLink(item.name, value: item)
            }
            .navigationDestination(for: Item.self) { item in
                DetailView(item: item)
            }
            .navigationTitle("Home")
        }
    }
}

struct DetailView: View {
    var item: Item
    
    var body: some View {
        VStack {
            Text("Details for: \(item.name)")
        }
        .navigationTitle("Detail View")
    }
}

struct Item: Identifiable, Hashable {
    let id = UUID()
    let name: String
}
```
Now, in the **NavigationLink** we are not passing a view, but data (in this case, of the *Item* type, although we could add more types).
Next, in the *navigationDestination(for:)* modifier we set the navigation. This is where it is possible to add different paths depending on the type passed to it. For example:

```swift
struct ContentView: View {
    let items = [Item(name: "Item 1"), Item(name: "Item 2"), Item(name: "Item 3")]
    let boxes = [Box(name: "Box 1"), Box(name: "Box 2"), Box(name: "Box 3")]
    
    var body: some View {
        NavigationStack {
            List {
                ForEach(boxes) { box in
                    NavigationLink(box.name, value: box)
                }
                ForEach(items) { item in
                    NavigationLink(item.name, value: item)
                }
            }
            .navigationDestination(for: Box.self) { box in
                BoxView(box: box)
            }
            .navigationDestination(for: Item.self) { item in
                ItemView(item: item)
            }
            .navigationTitle("Home")
        }
    }
}

struct ItemView: View {
    var item: Item
    
    var body: some View {
        VStack {
            Text("Details for: \(item.name)")
        }
        .navigationTitle("Item View")
    }
}

struct BoxView: View {
    var box: Box
    
    var body: some View {
        VStack {
            Text("Details for: \(box.name)")
        }
        .navigationTitle("Box View")
    }
}

struct Item: Identifiable, Hashable {
    let id = UUID()
    let name: String
}

struct Box: Identifiable, Hashable {
    let id = UUID()
    let name: String
}
```
Where if the item passed is of type *Item*, it will show a DetailedView, while for a type Box it will show a BoxView.

{{<ads2>}}
## Conclusion
NavigationStack is a game changer for Navigation in SwiftUI. allowing to upgrade our apps.
