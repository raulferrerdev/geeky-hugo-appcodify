---
title: "SwiftUI #5. Using the Button component"
description: "The Button view, which can be modified with a number of modifiers to change its appearance and behavior, is used in SwiftUI to build buttons."
date: 2022-12-26
categories: ["SwiftUI"]
tags: ["Development", "Code"]
image: "https://drive.google.com/uc?id=1KDGGuMdzk8mIuasmDDnsqQm8WvLsoUuG"
type: "regular" # available types: [featured/regular]
draft: false
---

{{< youtube nTnd9fNoukA >}}

Any app must include buttons in order for users to interact with the user interface and initiate actions. The **Button** view, which can be modified with a number of modifiers to change its appearance and behavior, is used in SwiftUI to build buttons.
{{<ads1>}}

Use the following code in SwiftUI to build a simple button:

```swift
Button(action: {
    // action to be performed when button is tapped
}) {
    Text("Button")
}
```

By doing this, a button with the label "Button" is created, and tapping it causes it to carry out the desired action. By nesting them inside the Text view, you may also utilize other views, like photos, as the label for the button.

# Modifiers

*ForegroundColor*, a modifier that lets you alter the color of the button's label, is one of the most helpful ones. As an illustration, the code that follows generates a red button with white text:

```swift
Button(action: {
    // action to be performed when button is tapped
}) {
    Text("Button")
}
.foregroundColor(.white)
.background(Color.red)
```

The font and size of the button's label can also be modified using the font modifier. For instance:

```swift
Button(action: {
    // action to be performed when button is tapped
}) {
    Text("Button")
}
.font(.system(size: 18))
```

*Padding*, another helpful modification, provides padding around the button's label. This is helpful for designing buttons with a roomier layout:


```swift
Button(action: {
    // action to be performed when button is tapped
}) {
    Text("Button")
}
.padding()
```

The *frame* modifier can also be used to modify the button's size. This is helpful for making buttons of a particular size:

```swift
Button(action: {
    // action to be performed when button is tapped
}) {
    Text("Button")
}
.frame(width: 220, height: 44)
```

SwiftUI offers a number of specific button kinds, such as **Toggle** and **Picker**, in addition to the standard Button view. You can use **Toggle** to build a toggleable button, and **Picker** to construct a button that displays a menu of options for the user to select from.

Here is a SwiftUI **Toggle** button illustration:

```swift
@State private var isOn = false

Toggle(isOn: $isOn) {
    Text("Toggle button")
}
```

And here's an example of a **Picker** button:

```swift
@State private var selectedOption = "Option A"

Picker("Options", selection: $selectedOption) {
    Text("Option A").tag("Option A")
    Text("Option B").tag("Option B")
    Text("Option C").tag("Option C")
}
```

You can also utilize the Button view to construct buttons with unique behaviors and designs in addition to these specific button types. You might, for instance, use the *onLongPressGesture* modifier to start an action when the button is held down for a predetermined period of time.

```swift
Button(action: {
    // action to be performed when button is tapped
}) {
    Text("Button")
}
.onLongPressGesture {
    // action to be performed when button is held down
}
```

We can also use the *buttonStyle modifier* to apply a custom style to your button.

```swift
struct RoundedButtonStyle: ButtonStyle {
    func makeBody(configuration: Configuration) -> some View {
        configuration.label
            .padding(10)
            .background(configuration.isPressed ? Color.gray : Color.blue)
            .foregroundColor(.white)
            .clipShape(RoundedRectangle(cornerRadius: 10))
    }
}

Button(action: {
    // action to be performed when button is tapped
}) {
    Text("Button")
}
.buttonStyle(RoundedButtonStyle())
```

The *disabled *modifier, which disables the button when set to true, and the *scaleEffect* modifier, which scales the button up or down when tapped, are just two of the many more choices available in SwiftUI for customizing buttons. You can design buttons that are specific to the requirements of your app by mixing these modifiers and developing unique looks and behaviors.

{{<ads2>}}
