---
title: "SwiftUI #11. Building Lists"
description: "In this chapter, we will delve deep into the world of Lists in SwiftUI, exploring the various features and capabilities of this important component. We will cover the basics of what a List is and how it works, the different types of modifiers you can use to customize your lists, how to create sections with headers and footers, and more."
date: 2023-02-07
categories: ["SwiftUI"]
tags: ["Development", "Code"]
image: "https://drive.google.com/uc?id=1JrKkBNt1sGQOOu5qXzYXWR3F1toMNc2Q"
type: "regular" # available types: [featured/regular]
draft: false
---

{{< youtube YpmvfYZlMtc >}}

A **List** is a SwiftUI compoent that displays a *vertically scrollable* collection of rows. This component is used to display data from arrays, dictionaries, and other data sources. Text, images, and other elements can also be included.
{{<ads1>}}

## How does a List function in SwiftUI

SwiftUI's **List** view, which takes an *array of data* as an argument, is used to create a **List**. The **List** view then generates rows for each item in the array automatically. A closure that returns a **View** is used to define each row. For each item in the array, the closure is called, and the view it returns is used to display that item.

For example:

```swift
struct ContentView: View {
   var body: some View {
        List {
            ForEach(0..<5) { item in
                Text("Item \(item)")
            }
        }
   }
}

```
In this example, the List view generates five rows, each one containing a Text view taht displays the number of the item.

## How to generate a List

We can use some methods to build a List:

* **List with closure.** We can use a closure to build the list elements in the List view. When we need to modify the elements before showing them, using this method is helpful. Is the example we've just seen:

```swift
struct ContentView: View {
   var body: some View {
        List {
            ForEach(0..<5) { item in
                Text("Item \(item)")
            }
        }
   }
}
```

* **List with custom view.** We can create the elements of a list using a custom view. Is usefule when we need to display more intricate or custom views as list components:

```swift
struct ContentView: View {
   var body: some View {
        List {
            CustomListElement(item: "Item 1")
            CustomListElement(item: "Item 2")
            CustomListElement(item: "Item 3")
        }
   }
}


struct CustomListElement: View {
    var item: String
   
    var body: some View {
        Text(item)
    }
}
```

* **List with an array of objects.** We can create the elements of a list using an array of unique objects. This is the case, for example, when displaying data from an external source, such as a server or database:

```swift
struct Item: Identifiable {
   var id: Int
   var name: String
}

struct ContentView: View {
   var items: [Item] = [
      Item(id: 1, name: "Item 1"),
      Item(id: 2, name: "Item 2"),
      Item(id: 3, name: "Item 3"),
   ]
   
   var body: some View {
      List(items) { item in
         Text(item.name)
      }
   }
}
```

## List modifiers
List come with some userful modifiers:

* ***.listStyle.*** Specifies the style of the list, such as *plain* or *grouped*.

```swift
struct ContentView: View {
    var body: some View {
        List {
            ForEach(0 ..< 5) { index in
                Text("Item \(index)")
            }
        }
        .listStyle(GroupedListStyle())
    }
}
```
In this case, we've used the *GroupedListStyle* style, but there is other like *PlainListStyle* or *InsetGroupedListStyle*.

* ***.onDelete.*** Specifies the action to take when an item is *deleted* from the list.

```swift
struct ContentView: View {
    var body: some View {
        List {
            ForEach(0..<5) { item in
                Text("Item \(item)")
            }
            .onDelete { indexSet in
                // Perform action when item is deleted
            }
        }
    }
}
```

* ***.onMove.*** Specifies the action to take when an item is *moved* within the list.

```swift
struct ContentView: View {
    @State private var items = ["Item 1", "Item 2", "Item 3", "Item 4", "Item 5"]
    
    var body: some View {
        List {
            ForEach(items, id: \.self) { item in
                Text(item)
            }
            .onMove { sourceIndex, destinationIndex in
                items.move(fromOffsets: sourceIndex, toOffset: destinationIndex)
            }
        }
    }
}
```

## Sections

In a **List**, a **section** is a collection of linked things. A **section** can have a **header** and a **footer**, and we can use **separators** to arrange and divide its items.

For example, we could make a list of contacts and categorize them according to the initial letter of their names. In this wai, data can be logically grouped using **sections**, which makes it simpler for consumers to comprehend and browse the data. 

To build **sections**, a ForEach view may be used to iterate over an array of **sections** to construct **sections** in a List view, and another ForEach view can be used to iterate over the items contained in each **section**.

```swift
struct ListSection: Identifiable {
    var id: Int
    var header: String
    var items: [String]
}

struct ContentView: View {
    var sections: [ListSection] = [
        ListSection(id: 0, header: "A", items: ["Alice", "Anna", "Adam"]),
        ListSection(id: 1, header: "B", items: ["Bob", "Beth", "Brad"]),
    ]
    
    var body: some View {
        List {
            ForEach(sections) { section in
                Section(header: Text(section.header)) {
                    ForEach(section.items, id: \.self) { item in
                        Text(item)
                    }
                }
            }
        }
    }
}
```

In addition to a *header*, we can also add a *footer* to a *Section*:

```swift
struct ContentView: View {
    var items = ["Item 1", "Item 2", "Item 3", "Item 4", "Item 5"]
    
    var body: some View {
        List {
            Section {
                ForEach(items, id: \.self) { item in
                    Text(item)
                }
            } header: {
                Text("List Header")
            } footer: {
                Text("List Footer")
            }
        }
    }
}
```
{{<ads2>}}

## Conclusion
*List* view is an effective tool for showing collections of data. You may make unique lists with its many features to suit the requirements of our app. The use of modifiers as *.deleted* or *.onMove* allow us to add some fuctionality. The addition of *headers* and *footers* allow us to give more information to the user on using our app.