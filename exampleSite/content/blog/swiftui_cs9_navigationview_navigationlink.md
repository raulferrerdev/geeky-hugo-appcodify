---
title: "SwiftUI #9. Navigation in SwiftUI: A Comprehensive Guide to NavigationView and NavigationLink"
description: "This post will go over how to use NavigationView and NavigationLink, which are the two main components for managing the navigation stack and navigating between views in an app. The post describes how to use these components in detail, as well as how to customize their appearance and behavior with built-in modifiers. The guide also includes examples and explanations of how to pass data between views and control navigation programmatically."
date: 2023-01-23
categories: ["SwiftUI"]
tags: ["Development", "Code"]
image: "https://drive.google.com/uc?id=10KAqSHojb16PiQE6iqKHIn4ni4lzuLI2"
type: "regular" # available types: [featured/regular]
draft: true
---

**NavigationView** and **NavigationLink** are key components in SwiftUI for managing the navigation stack and navigating between views in an app. **NavigationView** acts as a container view that manages the navigation stack, while **NavigationLink** pushes a new view onto the stack when tapped or triggered programmatically. Together, they allow for seamless navigation throughout an app.

## NavigationView
To use **NavigationView**, it should be added to the main body of a SwiftUI view. Inside the **NavigationView**, content such as a list of items that the user can tap to navigate to a detail view can be added.

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
            .background(Color.green)
        }
    }
}

struct DetailView: View {
    var item: Item

    var body: some View {
        VStack {
            Text("Details for: \(item.name)")
        }
        .navigationBarTitle("Detail View")
    }
}

struct Item {
    let name: String
}
```
The *ContentView* in this example has an array of items, each of which is displayed in a list, and each of which is enclosed in a **NavigationLink**, which, when clicked, directs the user to the *DetailView* while supplying the item as an argument. A *List* that is enclosed by a **NavigationView** contains the **NavigationLink**. There is a modifier for the *NavigationView*, *navigationBarTitle("Home")*, and for the **NavigationView**'s background, *.background(Color.green)*.

### NavigationView modifiers

**NavigationView** has several built-in *modifiers* that allow for customization of its appearance and behavior, including:

* **.navigationBarTitle(Text("Title"))**. Sets the title of the navigation bar.
* **.navigationBarHidden(Bool)**. Hide or show the navigation bar.
* **.navigationBarBackButtonHidden(Bool)**. Hide or show the back button in the navigation bar.
* **.navigationBarItems(leading:, trailing:)**. To add leading and trailing buttons to the navigation bar.
* **.toolbar(items:)**. To add items to the toolbar.
* **.background(Color)**. To set the background color of the **NavigationView**.
* **.transition(AnyTransition)**. To set the transition animation when navigating between views.
* **.navigationViewStyle(StackNavigationViewStyle())**. To set the navigation style of the **NavigationView**.
* **.edgesIgnoringSafeArea(.top)**. To make the **NavigationView** ignore the safe area at the top.

For example, to set the title of the navigation bar and hide the back button:
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
            .navigationBarHidden(false)
            .navigationBarBackButtonHidden(true)
            .background(Color.green)
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

struct Item {
    let name: String
}
```

In this example, the **NavigationView** has some modifiers: *.navigationBarTitle("Home")* to set the title of the navigation bar, *.navigationBarItems(leading:, trailing:)* to add leading and trailing buttons to the navigation bar, *.navigationBarHidden(false)* to show the navigation bar, *.navigationBarBackButtonHidden(true)* to hide the back button, and *.background(Color.green)* to change the background color of the **NavigationView**.

## NavigationLink
As we have seen, **NavigationLink** allows for navigating between views in an app by being added to a SwiftUI view and specifying the destination view to navigate to using the destination parameter.

```swift
struct ContentView: View {
    @State private var selectedId: String? = nil
    let items = [Item(id: "1", name: "Item 1"), Item(id: "2", name: "Item 2"), Item(id: "3", name: "Item 3")]

    var body: some View {
        NavigationView {
            List(items) { item in
                NavigationLink(destination: DetailView(), tag: item.id, selection: $selectedId) {
                    Text(item.name)
                }
            }
            .navigationBarTitle("Home")
        }
    }
}

struct DetailView: View {
    @Environment(\.selectedId) var selectedId

    var body: some View {
        VStack {
            Text("Selected item id: \(selectedId ?? "None")")
            NavigationLink(destination: ContentView(), isActive: .constant(false), label: {
                Text("Go back to Home")
            })
        }
        .navigationBarTitle("Detail View")
    }
}

struct Item {
    let id: String
    let name: String
}
```






### tag
The *tag* property of the **NavigationLink** allows for passing data to the destination view:

```swift
NavigationLink(destination: DetailView(item: item), tag: item.id, selection: $selectedId) {
    Text(item.name)
}
```
In the destination view, the passed data can be accessed using the environment variable.

```swift
struct DetailView: View {
    @Environment(.selectedId) var selectedId
    var item: Item
    var body: some View {
        Text(item.name)
    }
}
```
### isActive

The *isActive* property of the **NavigationLink** allows for programmatically controlling whether the link is active or not. This can be useful if you want to conditionally navigate to a view based on some logic. For example:

```swift
NavigationLink(destination: DetailView(), isActive: $showDetail) {
    Text("Show Detail View")
}
```

The *isActive* property is a *binding*, which means that you can change its value from elsewhere in your code to activate or deactivate the link. For example, you can set *showDetail = true* when a user taps a button, and the **NavigationLink** will navigate to the *DetailView*.
You can also use the **NavigationLink(destination:, isActive:, label:)** with *isActive* set to *false* and *label* to *.pop* to navigate back to the previous view.
```swift
NavigationLink(destination: Text("Hello World"), isActive: self.$isActive, label: { EmptyView() }).hidden()
```

## Conclusion
**NavigationView** and **NavigationLink** are essential components in SwiftUI for managing the navigation stack and navigating between views in an app. Understanding and utilizing the various *modifiers* available for **NavigationView** and **NavigationLink** allows for more customization and control over the navigation flow in your app.