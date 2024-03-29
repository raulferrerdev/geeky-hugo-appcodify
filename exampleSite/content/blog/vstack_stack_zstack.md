---
title: "SwiftUI #1. VStack, HStack and ZStack: SwitUI basic layout"
description: "Learn about the basic layouts in SwifUI with VStack, HStack, ZStack and Spacer."
date: 2022-12-17
categories: ["SwiftUI"]
tags: ["Development", "Code"]
image: "https://drive.google.com/uc?id=1TGpWa1YdCTo5JToV_EXuOyPDWV7wQdqe"
type: "regular" # available types: [featured/regular]
draft: false
---

SwiftUI allows us to easily compose a user interface (UI) by adding components (View) and organizing them in the UI space according to our interests.
The components to organize these Views are basically three: **VStack**, **HStack** and **ZStack**.
{{< youtube -dQ7ADkO_qk >}}
{{<ads1>}}

## VStack
When we create an application with SwiftUI, a default scene (*ContentView*) appears, which contains the following code:
```swift
struct ContentView: View {
    var body: some View {
        VStack {
            Image(systemName: "globe")
                .imageScale(.large)
                .foregroundColor(.accentColor)
            Text("Hello, world!")
        }
        .padding()
    }
}
```
If we run this code, we'll see that an icon of the world appears in the middle of the screen, and below it, the text *'Hello, world!'*. That is, we can see two elements (an *Image* element and a *Text* element) appearing centered in the image on top of each other.
This is because both elements are inside a **VStack** structure, which is to vertically place (a column) all the elements inside.
By default, components are ordered center-aligned with no gap between them.) But we can change this with the alignment and spacing parameters that we can assign to VStack:

```swift
VStack(alignment: .leading, spacing: 10) {
    ...
}
```
In this case, what we have done is say that the elements should be aligned to the left and with a separation between them of 10.
The main alignments that we can give are (*HorizontalAlignment* type): *.leading* (left), *.center* (center) and *.trailing* (right).

## HStack
Now we change **VStack** for **HStack**, we will see that the two elements have been placed next to each other (probably if you have directly changed **VStack** for **HStack** it will have given you an error, since the values that the align parameter accepts are different, except for *.center*).
```swift
struct ContentView: View {
    var body: some View {
        HStack {
            Image(systemName: "globe")
                .imageScale(.large)
                .foregroundColor(.accentColor)
            Text("Hello, world!")
        }
        .padding()
    }
}
```
For the *align* parameter, in **HStack** we can use *.top* (the elements are aligned by the top of the highest of them), *.center* (the centers are aligned) and *.bottom *(they are aligned by the bottom of the elements).
```swift
HStack(alignment: .top, spacing: 25) {  
    ...
}
```

## ZStack
If **VStack** allows us to align the elements on the Y axis and **HStack** on the X axis, **ZStack** allows us to put them on the Z axis, that is, stacked one above the other.
For example, we modify the code used so far and put a *Rectangle* component instead of the *Image* component:

```swift
struct ContentView: View {
    var body: some View {
        ZStack {
            Rectangle()
                 .fill(Color.red)
                 .frame(width: 300, height: 300)
            Text("Hello, world!")
        }
    }
}
```

With this we get an red square to appear on the screen and, superimposed, the text *'Hello, World!'*.

**ZStack** only presents the *align* property and this can have multiple values (a combination of the values we have seen for **VStack** and **HStack**): *.top*, *.center*, *.bottom*, *.leading*, *.topLeading*, *.bottomLeading*, *.trailing*, *.topTrailing*, *.bottomTrailing*.

The order of appearance in a **ZStack** is: the first in the list (in this case the rectangle), is the one that appears lowest in the stack. However, we can change the position of the elements regardless of their place in the list. This is done by adding the *zIndex* parameter to the stack components.

Thus, if we put *.zIndex(1)* to the rectangle, it would appear above the text (we would not see the text) even though the code is the first.

```swift
struct ContentView: View {
    var body: some View {
        ZStack {
            Rectangle()
                .fill(Color.red)
                .frame(width: 300, height: 300)
                .zIndex(1)
            Text("Hello, world!")
        }
    }
}
```

> By default, zIndex has a value of 0 on each element of a **ZStack**. This value can be changed to positive or negative values to modify the positioning on the stack.
{{<ads2>}}

## Spacer
We have seen that in the case of **VStack** and **HStack** we can indicate the separation between the different elements through the spacing parameter. But what if we wanted the **VStack** items to be at the bottom of the screen? And at the top?
For this we have the **Spacer** element. This is an element with no content that expands to take up all available space (vertically on a **VStack** and horizontally on an **HStack**).

```swift
struct ContentView: View {
    var body: some View {
        VStack {
            Spacer()
            Image(systemName: "globe")
                .imageScale(.large)
                .foregroundColor(.accentColor)
            Text("Hello, world!")
        }
        .padding()
    }
}
```
By putting the **Spacer** element first, what we'll do is make the *Image* component and the *Text* component scroll to the bottom of the screen.
