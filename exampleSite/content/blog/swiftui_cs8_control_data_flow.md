---
title: "SwiftUI #8. Control data flow with @State, @Binding, @ObserverObject, and @Published"
description: "This chapter covers the use of property wrappers in SwiftUI: @State, @Binding, @ObservedObject, and @Published. We will examine examples and use cases to understand how to effectively manage the state of views and objects, and create dynamic and reactive user interfaces."
date: 2023-01-20
categories: ["SwiftUI"]
tags: ["Development", "Code"]
image: "https://drive.google.com/uc?id=10KAqSHojb16PiQE6iqKHIn4ni4lzuLI2"
type: "regular" # available types: [featured/regular]
draft: false
---

{{< youtube mx2RNax0kMo >}}


One of the key features of SwiftUI is its use of **property wrappers**, which are used to control the flow of data through the view hierarchy. In this article, we will explore four **property wrappers** in particular: **@State**, **@Binding**, **@ObservedObject**, and **@Published**.
{{<ads1>}}

# @State
SwiftUI's **@State** property wrapper gives you the ability to control a view's state. The view will automatically be stored in its internal state when a property is designated with the **@State** wrapper, and any changes to the property will cause the view to be rendered again. The display will update immediately when the state changes, making it simple to design dynamic and reactive user interfaces.

Here is an example of a straightforward toggle button that controls its internal state using **@State**:

```swift
struct ContentView: View {
    @State private var isOn = false

    var body: some View {
        Button(action: {
            self.isOn.toggle()
        }) {
            Text(isOn ? "On" : "Off")
        }
    }
}
```

The **@State** wrapper is used in this example to indicate that the *isOn* property is saved in the internal state of the view. The *isOn* property's value is simply toggled by the button's action, and the view automatically changes to reflect the new value.

It's important to remember that **@State** attributes can only be accessed or changed within the view in which they are declared. This guarantees that a view's state is managed in a predictable and self-contained manner.

Another advantage is that each time the value is obtained, **@State** creates a brand-new duplicate of it. The goal of this so-called copy-on-write semantics is to maintain the consistency of the view's state. When various views might be reading or altering the same state variable, this can be useful.

The two text fields in this example each have the same text states.

```swift
struct ContentView: View {
    @State var text: String = ""
    var body: some View {
        VStack {
            TextField("Enter text:", text: $text)
            TextField("Enter text:", text: $text)
        }
    }
}
```
The **@State** attribute is shared by both of the text fields in this example, so any changes made to one will also be reflected in the other.

Additionally, it's important to remember that **@State** properties support a wide range of types, including custom *structs*, *classes*, *Strings*, *Ints*, and *Doubles*.

In conclusion, **@State** is a potent property wrapper that enables you to handle a view's state in a predictable and self-contained manner. The property is automatically stored in the view's internal state, a re-render of the view is triggered when the state changes, and a fresh copy of the value is created each time it is accessed to ensure consistency. You may design dynamic and reactive user interfaces that react to user interactions by updating automatically by using **@State**.

# @Binding
**@Binding** property wrapper enables you to share a value across many views in your app. In other words, any changes made to the state will automatically reflect in the view, and any changes made to the view will automatically reflect in the state, thanks to the usage of this technique to establish a two-way binding between the state and a view.

A child view can read the value of a property from a parent view and make it available for usage by the child view by using a **@Binding** property. As a result, data may be shared between views more effectively without a global state or repeated passing of the value down the view hierarchy.

By using **@Binding**, you can establish two-way communication between views so that whenever a value is changed in one, it automatically updates in the other. This can be helpful if you want to share a value across several views or allow a child view to make changes to a value in the parent view.

Here is an example of a parent view with a button that shows a modal view (as sheet):

```swift
struct ParentView: View {
    @State private var showModal: Bool = false
    var body: some View {
        Button(action: { self.showModal.toggle() }) {
            Text("Show Modal")
        }
        .sheet(isPresented: $showModal) {
            ModalView(showModal: $showModal)
        }
    }
}

struct ModalView: View {
    @Binding var showModal: Bool
    var body: some View {
        VStack {
            Text("This is a modal")
            Button(action: { self.showModal.toggle() }) {
                Text("Dismiss Modal")
            }
        }
    }
}
```
In this example, we have two views ParentView and ModalView. ParentView has a button that toggles the showModal state. When the button is pressed, it will toggle the showModal state, causing the ModalView to appear or disappear. We are passing the showModal property marked with @State in the ParentView to the ModalView as a **@Binding** property. This way any changes made to the showModal property in the ParentView will automatically be reflected in the ModalView and vice versa. When the "Dismiss Modal" button is pressed, it will toggle the showModal state and cause the modal to disappear.

The ability to build a two-way binding between the parent and child views is another feature of **@Binding**. This implies that the parent view can react to changes in the child view's state and modify its own state. Here is an example of a text field where the user can change a person's name:

In conclusion, **@Binding** is a potent property wrapper that enables data to be passed from a parent view to a child view. While it does not make a fresh copy of the data, it does allow the child view to read from and write to the parent's state.

In response to user interactions, this enables several views to update dynamically and share the same state. Additionally, it gives you more control over how data moves through the view hierarchy by enabling you to construct a two-way binding between the parent and child views. You may design more intricate and dynamic user interfaces that react in real-time to user interactions by utilizing **@Binding**.

# @ObservedObject
SwiftUI's **@ObservedObject** property wrapper enables you to control an object's state that complies with the *ObservableObject protocol*. Any modifications to an object that has the @ObservedObject wrapper will cause the view to be re-rendered. In response to changes in the underlying data, it is simple to design dynamic and reactive user interfaces that update automatically.

> **ObservableObject** enables to keep track of class modifications. Any views that depend on a class instance annotated with **ObservableObject** will reload themselves automatically to reflect the changing state. This enables a user interface that is more responsive and effective. To utilize **ObservableObject**, a class's properties must have an *objectWillChange* property that will be called before any changes are made to the class's properties. For example, the *objectWillChange* publisher is sent automatically by the **Published** property wrapper so that you don't have to explicitly call it each time a property changes.

Here is an example of a view that uses a **@ObservedObject** to show the current weather conditions:

```swift
class WeatherService: ObservableObject {
    @Published var temperature = 35.0
    @Published var condition = "Sunny"
}

struct WeatherView: View {
    @ObservedObject var weatherService = WeatherService()

    var body: some View {
        VStack {
            Text("Temperature: \(String(format: "%.1f", weatherService.temperature))ºC")
            Text("Condition: \(weatherService.condition)")
        }
    }
}
```

The WeatherService class in this example adheres to the *ObservableObject protocol*, and a WeatherView instance of the class is identified with the **@ObservedObject** wrapper. The Text views will automatically update to reflect the new values whenever changes are made to the temperature or condition attributes of the WeatherService, which will cause a re-render of the view.

It's important to remember that **@ObservedObject** attributes can only be accessed or changed within the view in which they are specified. This guarantees that a view's state is managed in a predictable and self-contained manner.

In conclusion, **@ObservedObject** is an effective property wrapper that enables you to control an object's state when it complies with the *ObservableObject protocol*. It is simple to construct dynamic and reactive user interfaces since it automatically initiates a re-render of the display when the object's state changes. 

Additionally, it gives you more control over how data flows through your app because it lets you manage the state of objects that are not a part of the display hierarchy. You can build more intricate and dynamic user interfaces that react in real-time to changes in the underlying data by using **@ObservedObject**.

# @Published
As we have just seen, SwiftUI's **@Published** property wrapper generates the *ObservableObject protocol*-compliant code for an object automatically. In addition to frequently being used in conjunction with the **@State** and **@Binding** wrappers, it can be used in place of the **@ObservedObject** wrapper. Any modifications to a property that has the **@Published** wrapper applied will cause the view to be re-rendered.

In this example, the name and age attributes of the Person class are marked with the **@Published** wrapper. The **@ObservedObject** wrapper is used by the PersonView to watch the object's state, and thus automatically creates the appropriate code for the class to comply with the *ObservableObject protocol*. The Text views will automatically update to reflect any changes made to the name or age properties of the individual because doing so will cause the view to be rendered again.

It's important to keep in mind that **@Published** properties are only accessible within the object they are declared in, and other objects cannot access or modify them. This guarantees that the object's state is managed in a controlled and predictable manner.

The ability to construct a two-way binding between the object and the view is another aspect of **@Published**. This implies that the state of the object can be changed by the view, and the object can react to those changes. Here is an example of a text field where the user can change a person's name:

```swift

class Person: ObservableObject {
    @Published var name = "John Doe"
    @Published var age = 30
}

struct PersonView: View {
    @ObservedObject var person: Person

    var body: some View {
        VStack {
            TextField("Enter name", text: $person.name)
            Text("Name: \(person.name)")
            Text("Age: \(person.age)")
        }
    }
}
```
The **$** prefix in this example establishes a binding between the text field and the person's name property. The person's name property updates when the user inputs in the text box, and the view responds by changing the Text view.

In conclusion, **@Published** is a property wrapper that may be used in place of **@ObservedObject** and is frequently combined with the **@State** and **@Binding** wrappers to automatically make an object comply to the ObservableObject protocol. It makes it simple to design dynamic and reactive user interfaces since it enables you to manage the state of an object and causes a re-render of the view when the object's state changes.
{{<ads2>}}


# Conclusion
This chapter has explored the use of **property wrappers** extensively, with a focus on the **@State**, **@Binding**, **@ObservedObject**, and **@Published** wrappers.

Using these wrappers, we have learned how to control the state of views and objects, send data between views, and create dynamic and reactive user interfaces.

We have also seen examples and use cases of how to successfully use these wrappers in our own applications. With the help of this knowledge, you will be able to create user interfaces that are more complex and dynamic and respond instantly to user input and data changes.