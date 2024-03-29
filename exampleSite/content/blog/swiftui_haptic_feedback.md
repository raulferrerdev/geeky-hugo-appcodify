---
title: "Adding Haptic Feedback in SwiftUI"
description: "In this tutorial, we'll explore how to create a custom view modifier in SwiftUI that adds haptic feedback. We'll see how to use the UIImpactFeedbackGenerator, UINotificationFeedbackGenerator, and UISelectionFeedbackGenerator to provide tactile responses to user interactions."
date: 2023-03-04
categories: ["SwiftUI", "Design"]
tags: ["Development", "Code"]
image: "https://drive.google.com/uc?id=11AznL5JKLNaxDDl52oazasuCrsZqOtz-"
type: "regular" # available types: [featured/regular]
draft: false
---

# What is haptic feedback?
Surely you have noticed in an application how pressing a button or moving a swift produces a slight vibration. This information that we receive in this way from the application is called **haptic feedback**.
This feedback can be in the form of vibrations, touches, or other physical sensations that the user can feel through the device.
With this **haptic feedback** we can improve the user experience and make your app feel more responsive and engaging. Let's now see how to use **haptic feedback** in SwiftUI.
{{<ads1>}}

# Using haptic feedback in SwiftUI
Although SwiftUI has **haptic feedback** itself, we can use it by adding UIKit and **CoreHaptics**, and then using the **UIFeedbackGenerator** class.

This class provides several different types of **haptic feedback**, including:

* **UIImpactFeedbackGenerator.** Provides feedback when the user performs an action of the impact type, such as touching a button.
* **UINotificationFeedbackGenerator.** Provide feedback when the user receives a notification or other type of alert.
* **UISelectionFeedbackGenerate.** Provides feedback when the user selects an item or interacts with a control.

Let's take a look at how to use each of type of **haptic feedback** in SwiftUI.

## UIImpactFeedbackGenerator

**UIImpactFeedbackGenerator** provides three different types of feedback: .*light*, .*medium*, and .*heavy*. Each type of feedback is designed to convey a different level of intensity to the user.

* **.light.** Indicates a light intreraction, such as tapping a button or scrolling to the end of a list.

* **.medium.** Indicates a medium interaction, such as pulling down on a refresh control or sliding a switch.

* **.heavy.** Indicates a heavy impact or collision, such as force-pressing a button or dropping an object.

In general, we should use *UIImpactFeedbackGenerator* to provide tactile feedback to the user when interacts with app's controls or objects. This can help to make the app feel more responsive and intuitive to use (**tip**: remember not overusing it, as too much haptic feedback can be distracting or annoying to the user).

To use the **UIImpactFeedbackGenerator**, you first need to create an instance of the class:

```swift
import SwiftUI

struct ContentView: View {
    let impactFeedback = UIImpactFeedbackGenerator(style: .medium)
    
    init() {
        impactFeedback.prepare()
    }
    
    var body: some View {
        Button(action: {
            impactFeedback.impactOccurred()
        }) {
            Text("Tap me")
        }.frame(width: 200, height: 50)
            .background(Color.blue)
            .foregroundColor(Color.white)
    }
}
```

In this example, we first create an instance of **UIImpactFeedbackGenerator** that we apply a *medium* style to when initializing it (this value provides a moderate amount of feedback). We then call the prepare method to prepare the feedback generator for use.

Next, we created a button that triggers *impactFeedback* when tapped. Tapping the button calls the *impactOccurred* method of the *impactFeedback* instance, which triggers the feedback. At that moment we will notice a slight vibration in the physical device (not in the simulator).

## UINotificationFeedbackGenerator

**UINotificationFeedbackGenerator** is used to provide haptic feedback in response to a notification (like indicate to the user that an event has occurred or to provide confirmation that an action has been completed).

**UINotificationFeedbackGenerator** provides three different types of feedback: .*success*, .*warning*, and .*error*:

* **.success.** Indicates that a task has completed successfully, or that the user has accomplished something. For example,  when the user completes a level in a game, or when they successfully submit a form.

* **.warning.** Indicates that the user has done something that requires their attention or action. For example, when the user tries to perform an action that is not permitted, or when there is an issue with their internet connection.

* **.error.** Indicates that an error has occurred or that something has gone wrong. For example, when the user enters invalid input or when there is a problem with the app's functionality.

Now, let's write a simple example (similar to previous one):

```swift
import SwiftUI

struct ContentView: View {
    let notificationFeedback = UINotificationFeedbackGenerator()
    
    init() {
        notificationFeedback.prepare()
    }
    
    var body: some View {
        Button(action: {
            notificationFeedback.notificationOccurred(.success)
        }) {
            Text("Show success message")
        }.frame(width: 200, height: 50)
            .background(Color.blue)
            .foregroundColor(Color.white)
    }
}
```
In this case, we have created an instance of **UINotificationFeedbackGenerator**, and then we have called de the *prepare* method to prepare it for use. In the *body*, we have added a Button that triggers the notification feedback when tapped. Because that, when the user taps the button, the *notificationOccurred* method of our *notificationFeedback* instance is called, which is in charge of trigger the feedback (for this example the *.success* style, which provides a feedback that indicates success).

## UISelectionFeedbackGenerator

**UISelectionFeedbackGenerator** is typically used to indicate that the user has moved the focus to a new control or object in the app, and only provides a single type of feedback: .*selection*. As a rule of thumb, we will use **UISelectionFeedbackGenerator** to provide tactile feedback to the user when interacts with the app controls or objects that involve selecting or changing focus.

Let's see an example:

```swift
import SwiftUI

struct ContentView: View {
    let selectionFeedback = UISelectionFeedbackGenerator()
    
    init() {
        selectionFeedback.prepare()
    }
    
    @State var isOn = false
    
    var body: some View {
        Toggle("Toggle me", isOn: $isOn)
            .onChange(of: isOn) { newValue in
                selectionFeedback.selectionChanged()
            }
            .padding()
    }
}
```

In this example, we created an instance of UISelectionFeedbackGenerator and called the prepare method to prepare it. Then, we have put a Toggle component that triggers the selection feedback when the user interacts with it. When the user toggles the switch, the onChange closure is called, and then the selectionChanged triggers the feedback.

# Converting haptic feedback into a reusable view modifier
In order to simplify the code of our application when we use haptic feedback, we can create a reusable view modifier. Let's see how.

First of all, we need to create the modifier (we well call it *HapticFeedbackModifier*). This modifier will take a parameter (*feedbackType*) of type *UINotificationFeedbackGenerator.FeedbackType*. Then, in the *body* function of the view modifier we will use the *onChange* method to trigger the feedback each time the view changes.

```swift
struct HapticFeedbackModifier: ViewModifier {
    let feedbackType: UINotificationFeedbackGenerator.FeedbackType
    
    func body(content: Content) -> some View {
        content.onChange(of: true) { _ in
            let feedback = UINotificationFeedbackGenerator()
            feedback.prepare()
            feedback.notificationOccurred(feedbackType)
        }
    }
}
```
Then, we need to add an extension to the View protocol to provide a *hapticFeedback* method. This method takes a *feedbackType* parameter and returns a modified version of the original view with the *HapticFeedbackModifier* applied.

```swift
extension View {
    func hapticFeedback(_ feedbackType: UINotificationFeedbackGenerator.FeedbackType) -> some View {
        self.modifier(HapticFeedbackModifier(feedbackType: feedbackType))
    }
}
```

Now you can use the hapticFeedback view modifier in your SwiftUI views like this:

```swift
struct ContentView: View {
    @State var isOn = false
    
    var body: some View {
        Toggle("Toggle me", isOn: $isOn)
            .hapticFeedback(.success)
            .padding()
    }
}
```
{{<ads2>}}

# Conclusion
In this post, we've explored how to use haptic feedback in SwiftUI, and their different types: UIImpactFeedbackGenerator, UINotificationFeedbackGenerator, and UISelectionFeedbackGenerator. On adding haptic feedback to our apps, we can enhance the user experience and make apps feel more responsive and engaging.
