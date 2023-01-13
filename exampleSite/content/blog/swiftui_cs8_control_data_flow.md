---
title: "SwiftUI #8. Control data flow with @State, @Binding, @ObserverObject, and @Published"
description: "This chapter covers the use of property wrappers in SwiftUI: @State, @Binding, @ObservedObject, and @Published. We will examine examples and use cases to understand how to effectively manage the state of views and objects, and create dynamic and reactive user interfaces."
date: 2023-01-15
categories: ["SwiftUI"]
tags: ["Development", "Code"]
image: "https://drive.google.com/uc?id=10KAqSHojb16PiQE6iqKHIn4ni4lzuLI2"
type: "regular" # available types: [featured/regular]
draft: true
---

//// DRAFT
SwiftUI is a modern, declarative framework for building user interfaces on Apple platforms. One of the key features of SwiftUI is its use of property wrappers, which are used to control the flow of data through the view hierarchy. In this article, we will explore four property wrappers in particular: @State, @Binding, @ObservedObject, and @Published.

Explain mor in detail @state, with advanced content and examples
@State is a property wrapper in SwiftUI that allows you to manage the state of a view. When a property is marked with the @State wrapper, it is automatically stored in the view's internal state and any changes to the property will trigger a re-render of the view. This makes it easy to create dynamic and reactive user interfaces, as the view will automatically update when the state changes.

Here is an example of a simple toggle button that uses @State to manage its internal state:

Copy code
struct ToggleButton: View {
    @State private var isOn = false

    var body: some View {
        Button(action: {
            self.isOn.toggle()
        }) {
            Text(isOn ? "On" : "Off")
        }
    }
}
In this example, the isOn property is marked with the @State wrapper, which means that it is stored in the view's internal state. The button's action simply toggles the value of the isOn property, and the view updates automatically to reflect the new value.

It's important to note that @State properties are only available within the view they are declared in, so they can't be accessed or modified by other views. This ensures that the state of a view is managed in a self-contained and predictable way.

Another feature of @State is that it creates a new copy of the value each time it is accessed. This is known as copy-on-write semantics and is designed to help maintain the consistency of the view's state. This can be useful in situations where multiple views may be reading or modifying the same state value.

Here's an example of two text fields that share the same state for their text contents.

Copy code
struct ContentView: View {
    @State var text: String = ""
    var body: some View {
        VStack {
            TextField("Enter text", text: $text)
            TextField("Enter text", text: $text)
        }
    }
}
In this example, both text fields are bound to the same @State property, which means that any changes made to one text field will be reflected in the other.

It's also worth noting that @State properties can be used with various types, including Strings, Ints, Doubles, and even custom structs and classes.

In summary, @State is a powerful property wrapper that allows you to manage the state of a view in a self-contained and predictable way. It automatically stores the property in the view's internal state, triggers a re-render of the view when the state changes, and creates a new copy of the value each time it is accessed to maintain consistency. By using @State, you can create dynamic and reactive user interfaces that update automatically in response to user interactions.




raul.ferrer.dev@icloud.com
Explain mor in detail @binding, with advanced content and examples
@Binding is a property wrapper in SwiftUI that allows you to pass data from a parent view to a child view. It allows the child view to read and write to the parent's state, but does not create a new copy of the data. This allows multiple views to share the same state and update dynamically in response to user interactions.

Here is an example of a parent view that contains a label and a button, and a child view that displays the label's text:

Copy code
struct ChildView: View {
    @Binding var text: String

    var body: some View {
        Text(text)
    }
}

struct ParentView: View {
    @State private var labelText = "Hello, World!"

    var body: some View {
        VStack {
            ChildView(text: $labelText)
            Button(action: {
                self.labelText = "Goodbye, World!"
            }) {
                Text("Change Label Text")
            }
        }
    }
}
In this example, the ChildView accepts a binding to a String property called text. The ParentView creates a @State property called labelText and passes it to the child view using the $ prefix, which creates a binding. This allows the child view to read and write to the parent's state, so when the button is tapped, the label text updates automatically in the child view.

It's important to note that @Binding properties are only available within the view they are passed to, so they can't be accessed or modified by other views. This ensures that the state is managed in a predictable way and that the parent view has full control over the state.

Another feature of @Binding is that it allows you to create a two-way binding between the parent and child views. This means that the child view can modify the parent's state, and the parent view can respond to those changes. Here is an example of a text field that allows the user to update the name of a person:

Copy code
struct PersonView: View {
    @ObservedObject var person: Person

    var body: some View {
        VStack {
            TextField("Enter name", text: $person.name)
            Text("Name: \(person.name)")
        }
    }
}
In this example, the text field is bound to the person's name property using a @Binding property wrapper. When the user types in the text field, the person's name property updates, and the parent view responds by updating the Text view.

In summary, @Binding is a powerful property wrapper that allows you to pass data from a parent view to a child view. It allows the child view to read and write to the parent's state, but does not create a new copy of the data. This allows multiple views to share the same state and update dynamically in response to user interactions. It also allows you to create a two-way binding between the parent and child views, giving you more control over the flow of data through the view hierarchy. By using @Binding, you can create more complex and dynamic user interfaces that respond to user interactions in real-time.




raul.ferrer.dev@icloud.com
Explain mor in detail @observedobject, with advanced content and examples
@ObservedObject is a property wrapper in SwiftUI that allows you to manage the state of an object that conforms to the ObservableObject protocol. When an object is marked with the @ObservedObject wrapper, any changes to the object will trigger a re-render of the view. This makes it easy to create dynamic and reactive user interfaces that update automatically in response to changes in the underlying data.

Here is an example of a simple view that displays the name and age of a person using an @ObservedObject:

Copy code
class Person: ObservableObject {
    var name = "John Doe"
    var age = 30
}

struct PersonView: View {
    @ObservedObject var person = Person()

    var body: some View {
        VStack {
            Text("Name: \(person.name)")
            Text("Age: \(person.age)")
        }
    }
}
In this example, the Person class conforms to the ObservableObject protocol, and an instance of the class is marked with the @ObservedObject wrapper in the PersonView. Any changes made to the person's name or age properties will trigger a re-render of the view, so the Text views will update automatically to reflect the new values.

It's important to note that @ObservedObject properties are only available within the view they are declared in, so they can't be accessed or modified by other views. This ensures that the state of a view is managed in a self-contained and predictable way.

Another feature of @ObservedObject is that it allows you to manage the state of objects that are not part of the view hierarchy. This can be useful in situations where you need to manage the state of an object that is not directly related to the view, such as a data model or a networking service. Here is an example of a view that displays the current weather conditions using an @ObservedObject:

Copy code
class WeatherService: ObservableObject {
    @Published var temperature = 72.0
    @Published var condition = "Sunny"
}

struct WeatherView: View {
    @ObservedObject var weatherService = WeatherService()

    var body: some View {
        VStack {
            Text("Temperature: \(weatherService.temperature)")
            Text("Condition: \(weatherService.condition)")
        }
    }
}
In this example, the WeatherService class conforms to the ObservableObject protocol, and an instance of the class is marked with the @ObservedObject wrapper in the WeatherView. Any changes made to the temperature or condition properties of the WeatherService will trigger a re-render of the view, so the Text views will update automatically to reflect the new values.

In summary, @ObservedObject is a powerful property wrapper that allows you to manage the state of an object that conforms to the ObservableObject protocol. It automatically triggers a re-render of the view when the object's state changes, making it easy to create dynamic and reactive user interfaces. It also allows you to manage the state of objects that are not part of the view hierarchy, giving you more control over the flow of data through your app. By using @ObservedObject, you can create more complex and dynamic user interfaces that respond to changes in the underlying data in real-time.




raul.ferrer.dev@icloud.com
Explain mor in detail @published, with advanced content and examples
@Published is a property wrapper in SwiftUI that automatically generates the necessary code for an object to conform to the ObservableObject protocol. It can be used in place of the @ObservedObject wrapper, and is often used in combination with the @State and @Binding wrappers. When a property is marked with the @Published wrapper, any changes to the property will trigger a re-render of the view.

Here is an example of a simple view that displays the name and age of a person using the @Published wrapper:

Copy code
class Person: ObservableObject {
    @Published var name = "John Doe"
    @Published var age = 30
}

struct PersonView: View {
    @ObservedObject var person = Person()

    var body: some View {
        VStack {
            Text("Name: \(person.name)")
            Text("Age: \(person.age)")
        }
    }
}
In this example, the Person class uses the @Published wrapper to mark its name and age properties. This automatically generates the necessary code for the class to conform to the ObservableObject protocol, and the PersonView uses the @ObservedObject wrapper to observe the object's state. Any changes made to the person's name or age properties will trigger a re-render of the view, so the Text views will update automatically to reflect the new values.

It's important to note that @Published properties are only available within the object they are declared in, so they can't be accessed or modified by other objects. This ensures that the state of the object is managed in a self-contained and predictable way.

Another feature of @Published is that it allows you to create a two-way binding between the object and the view. This means that the view can modify the object's state, and the object can respond to those changes. Here is an example of a text field that allows the user to update the name of a person:

Copy code
struct PersonView: View {
    @ObservedObject var person: Person

    var body: some View {
        VStack {
            TextField("Enter name", text: $person.name)
            Text("Name: \(person.name)")
        }
    }
}
In this example, the text field is bound to the person's name property using the $ prefix, which creates a binding. When the user types in the text field, the person's name property updates, and the view responds by updating the Text view.

In summary, @Published is a property wrapper that allows you to automatically make an object conform to the ObservableObject protocol, it can be used in place of the @ObservedObject wrapper, and is often used in combination with the @State and @Binding wrappers. It allows you to manage the state of an object and triggers a re-render of the view when the object's state changes, making it easy to create dynamic and reactive user interfaces. It also allows you to create a two-way binding between the object and the view, giving you more control over the flow of data through your app. By using @Published, you can create more complex and dynamic user interfaces that respond to changes in the underlying data in real-time.




## Conclusion
The use of property wrappers in SwiftUI has been extensively covered in this chapter, with an emphasis on the @State, @Binding, @ObservedObject, and @Published wrappers. We have learnt how to manage the state of views and objects, transmit information between views, and build dynamic and reactive user interfaces using these wrappers. Additionally, we have seen examples and use cases of how to use these wrappers successfully in our own applications. With this information, you will be able to design user interfaces that are more intricate and dynamic and react in real-time to user interactions and data changes.