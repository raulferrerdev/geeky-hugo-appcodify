---
title: "SwiftUI #13. LazyVGrid and LazyHGrid"
description: "In SwiftUI, LazyVGrid and LazyHGrid make it easy to create flexible and responsive grid layouts for iOS apps, with intuitive syntax and powerful features."
date: 2023-02-24
categories: ["SwiftUI"]
tags: ["Development", "Code"]
image: "https://drive.google.com/uc?id=15ByBxtsVy4oxCI-eJWImfWLG3bswxlnq"
type: "regular" # available types: [featured/regular]
draft: false
---

{{< youtube 0AOp4PeoR9k >}}

# Introduction
One of SwiftUI's most powerful features is the ability to lay out collections of views using flexible grid systems. Two of the most popular grid systems in SwiftUI are **LazyVGrid** and **LazyHGrid**. In this chapter, we'll take a deep dive into **LazyVGrid** and **LazyHGrid** and look at how they can be used to create complex and responsive user interfaces.
{{<ads1>}}

# LazyVGrid
**LazyVGrid** is a SwiftUI view that organizes its child views into a vertical grid. It provides a flexible and scalable method of displaying items in a grid-like format, with each row containing a different number of items and different items having different sizes.

To use **LazyVGrid**, we must first create an instance of this component and pass it an array of columns.
Each column specifies the size of an item in that column, which can be determined by the parameter passed. To do this we pass a GridItem component with the type of size:
```swift
struct ContentView: View {
    var items = 1...10
    var body: some View {
        LazyVGrid(columns: [GridItem(.fixed(100)), GridItem(.fixed(100))]) {
            ForEach(items, id: \.self) { item in
                Text("Item \(item)")
            }
        }
    }
}
```
In this example, we will make a **LazyVGrid** with two columns, each of which has a fixed width of 100 points. The grid will show ten items, each with a width of 100 points, resulting in five rows of two items each. 

## GridItem

**GridItem** is a SwiftUI struct that defines the size and behavior of the grid column (or row, as we will see later). It has options such as fixed size, adaptive size, and flexible size, allowing us to create custom and complex grid structures that adapt to the available space:

* **.fixed(CGFloat).** A fixed size in points, where the size is represented by the CGFloat value. This option comes in handy when you need to specify the exact size of a column or row.

* **.adaptive(CGFloat minimum, CGFloat maximum).** An adaptive size that adjusts based on available space, with the minimum and maximum values representing the minimum and maximum point size. This option is useful when you want a column or row to be adaptable and change size depending on available space.

* **.flexible().** A size that can expand or contract to fit the available space. This is useful when you want a column or row to take up all of the available space.

For example, with *.adaptive* we cancreate columns that will adjust their size to fit the available space:
```swift
struct ContentView: View {
    var items = 1...10
    var body: some View {
        LazyVGrid(columns: [GridItem(.adaptive(minimum: 50, maximum: 150))]) {
            ForEach(items, id: \.self) { item in
                Text("Item \(item)")
            }
        }
    }
}
```

In this example, we'll make a **LazyVGrid** with a single column that can change its size between 50 and 150 points depending on the available space. As a result, the grid can display items in a compact format when there isn't much space available, or in a more spacious format when there is.

With all this parameters in mind, we can create grids that perfectly match the needs of your app by combining different **GridItem** objects in the columns and rows parameters.

# LazyHGrid
In contrast to **LazyVGrid**, **LazyHGrid** arranges its childs views into a horizontal grid. **LazyHGrid** uses rows rather than columns but has the same fundamental functionality and parameters as **LazyVGrid**:

```swift
struct ContentView: View {
    var items = 1...10
    var body: some View {
        LazyHGrid(rows: [GridItem(.fixed(100)), GridItem(.fixed(100))]) {
            ForEach(items, id: \.self) { item in
                Text("Item \(item)")
            }
        }
    }
}
```

In this case, a **LazyHGrid** with two rows and a set height of 100 points is created. Ten items will be displayed in the matrix, with each having a height of 100 points. This will create five columns with two items each.

# Using LazyVGrid and LazyHGrid together
Combining **LazyVGrid** and **LazyHGrid** allows us to design intricate grid layouts that adjust to the available area. Consider using a **LazyHGrid** within each column to arrange items into rows after using a **LazyVGrid** to group items into columns.

For example:
```swift
    let items = 1...20
    let columns = [
        GridItem(.fixed(50)),
        GridItem(.flexible()),
        GridItem(.adaptive(minimum: 100, maximum: 200)),
        GridItem(.flexible()),
        GridItem(.fixed(50))
    ]
    
    let rows = [
        GridItem(.adaptive(minimum: 50, maximum: 100)),
        GridItem(.adaptive(minimum: 100, maximum: 150)),
        GridItem(.adaptive(minimum: 150, maximum: 200)),
        GridItem(.adaptive(minimum: 200, maximum: 250))
    ]
    
    var body: some View {
        LazyVGrid(columns: columns, spacing: 10) {
            ForEach(items, id: \.self) { item in
                LazyHGrid(rows: rows, spacing: 10) {
                    Text("\(item)")
                        .frame(maxWidth: .infinity, maxHeight: .infinity)
                        .background(Color.blue)
                        .foregroundColor(.white)
                }
            }
        }
        .padding()
    }
}
```

In this example we have a **LazyVGrid** with five columns, where the first and last columns have a fixed width of 50 points, the third column has a minimum width of 100 points and a maximum width of 200 points, and the second and fourth columns are flexible and adapt to the available space. The grid also has a 10 point spacing between the columns.

We also have a **LazyHGrid** with four rows inside each item of the **LazyVGrid**, with each row having an adaptive height ranging from 50 to 250 points. The grid also has a row spacing of 10 points.

Each cell of the LazyHGrid has a Text view inside that displays the item number. The text view has a blue background with white text and is sized to fill the entire cell using *frame(maxWidth:.infinity, maxHeight:.infinity)*.
{{<ads2>}}

# Conclusion
**LazyVGrid** and **LazyHGrid** are robust and adaptable grid systems that let design intricate and dynamic user interfaces. They offer a simple method for presenting collections of views in a grid-like format and adjust to the available area to guarantee that your layouts are consistently optimized for the user's device and screen size.