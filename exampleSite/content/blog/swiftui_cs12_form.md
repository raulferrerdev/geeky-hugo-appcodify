---
title: "SwiftUI #12. Creating Forms in SwiftUI"
description: "SwiftUI makes it simple to build forms that collect user input. This article will discuss sophisticated methods for modifying the look and behavior of forms in SwiftUI, such as creating original form controls, organizing controls into sections, and employing practical modifiers."
date: 2023-02-12
categories: ["SwiftUI"]
tags: ["Development", "Code"]
image: "https://drive.google.com/uc?id=1YjjmUR9RLPduOd74THQVgKsqB7D8XKug"
type: "regular" # available types: [featured/regular]
draft: false
---
{{< youtube PYi5LwZJgOU >}}


A typical UI component that may be used to collect user inputs in a systematic manner is the form. We'll explore advanced **SwiftUI** form-building tools and strategies in this tutorial.
{{<ads1>}}

## What's a SwiftUI Form
A **Form** in **SwiftUI** is a container view that arranges its child views into a single column. By default, the form is scrollable, allowing the user to view all of the material if it fills the entire screen height.

```swift
Form {
    TextField("Username", text: $username)
    TextField("Password", text: $password)
    TextField("Street", text: $street)
    TextField("City", text: $city)
    TextField("Zip code", text: $zipCode)
}
```

In this example, the created form has five TextField elements, all related to a common user sign up form: username, password, street...

## Using Section to group controls

As we saw in the [chapter of SwiftUI Lists](https://raulferrer.dev/blog/swiftui_cs11_list/), the **Section** view in SwiftUI can be used to group related controls together. The **Section** view accepts an optional header parameter for displaying a header above the controls. For instance, you could design a form with two sections like this:

```swift
Form {
    Section(header: Text("Account Information")) {
        TextField("Username", text: $username)
        SecureField("Password", text: $password)
    }

    Section(header: Text("Address")) {
        TextField("Street", text: $street)
        TextField("City", text: $city)
        TextField("Zip code", text: $zipCode)
    }
}
```
So, in this case, we have grouped the five TextField elements from the previous example into to sections: one for the account information (username and password) and the second, for the address input.

## Changing the appearance of Forms
When it comes to customizing the appearance of forms, **SwiftUI** offers a [lot of options](https://raulferrer.dev/blog/swiftui_ch3_vstack_modifiers/). For example, you can change the form's background color, padding, and even add custom dividers between sections.

```swift
Form {
    Section(header: Text("Account Information")) {
        TextField("Username", text: $username)
        SecureField("Password", text: $password)
    }

    Section(header: Text("Address")) {
        TextField("Street", text: $street)
        TextField("City", text: $city)
        TextField("Zip code", text: $zipCode)
    }
}
.background(Color.yellow)
.padding()
```

Remember that **SwiftUI** has a lot of modifiers to custom appearance:

* **.background.** Changes the background color or image of the form.

* **.padding.** Adds padding around the edges of the form.

* **.frame.** Changes the size and position of the form within its parent view.

* **.shadow.** Adds a shadow to the form.

* **.cornerRadius.** Changes the corner radius of the form.

* **.clipShape.** Clips the form to a custom shape.

* **.disabled.** Disables the form, making it unresponsive to user interaction.

* **.opacity.** Changes the opacity of the form.

* **.hidden.** Hides the form if set to true.

## Adding custom Form controls
In addition to the **SwiftUI** built-in controls, you can also create custom form controls in **SwiftUI**. For example, let's create a custom toggle that displays the label to the right of the toggle instead of to the left. To do this, you can use the HStack and Toggle views:
```swift
    struct CustomToggle: View {
    @Binding var isOn: Bool
    var label: String

    var body: some View {
        HStack {
            Toggle(isOn: $isOn)
            Text(label)
        }
    }
}
```
Then, we add this custom control to our **Form**:

```swift
Form {
    Section(header: Text("Account Information")) {
        TextField("Username", text: $username)
        SecureField("Password", text: $password)
    }

    Section(header: Text("Address")) {
        TextField("Street", text: $street)
        TextField("City", text: $city)
        TextField("Zip code", text: $zipCode)
    }

    Section(header: Text("Preferences")) {
        CustomToggle(isOn: $receiveEmails, label: "Receive emails")
        CustomToggle(isOn: $receiveNotifications, label: "Receive notifications")
    }
}
```
{{<ads2>}}

## Conclusion
**Forms** are an essential component of any app that requires user input, and creating forms with **SwiftUI** has never been easier. You can group related controls into sections, change the form's appearance, and even create custom form controls. I hope this article has helped you better understand **SwiftUI**'s advanced features and techniques for creating forms.