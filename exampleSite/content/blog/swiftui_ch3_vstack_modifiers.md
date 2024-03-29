---
title: "SwiftUI #3. Layout modifiers: frame, padding, background... and more"
description: "We are going to see some of the most used modifiers that can be applied to a View in SwiftUI."
date: 2022-12-21
categories: ["SwiftUI"]
tags: ["Development", "Code"]
image: "https://drive.google.com/uc?id=11oCQ5IZvxf6xOoL_RPAnAHz74gLXCBLs"
type: "regular" # available types: [featured/regular]
draft: false
---
{{< youtube Q2m3bpgI5gs >}}

In SwiftUI, methods called **view modifiers** can be chained to views to change their behavior or look. A view can be modified with view modifiers, which also make it easier to reuse code.

A view modifier can be used, for instance, to modify the typeface or add a shadow to a view. Modifiers of the view are written in the form. In order to make many changes to a single view, you can chain together several view modifiers using *.modifierName()*. Let's see some of the most used.
{{<ads1>}}

# .background(_:)
The *.background* modifier allows us to add one view as the background of another. The simplest example, and which is usually the use that is given the most, is to add a background color. For example:

```swift
Text("Hello, world!")
    .background(Color.red)
```

With this what we get is the text 'Hello, world!' appears on a red background (occupying the same space as the text).
But we can also add a view as a background. So, for example, we can add a blue rectangle:

```swift
Text("Hello, world!")
    .background(Rectangle().fill(Color.blue))
```
> According to [Apple documentation](https://developer.apple.com/documentation/swiftui/view-deprecated), the *background(_:alignment:)* method is deprecated as of iOS 16.2. *background(alignment:content)* should be used.

# .foregroundColor(_:)
The *.foregroundColor* modifier allows us to set the style of the elements that we put in the foreground, such as text, symbols or shapes.

```swift
Text("Hello, world!")
    .foregroundColor(Color.blue)
```

So the text 'Hello, world!' will be written in blue letters.

## .frame(_:)
With *.frame* we can give measurements to a view, such as its height, its width or its alignment:
```swift
func frame(
    width: CGFloat? = nil,
    height: CGFloat? = nil,
    alignment: Alignment = .center
) -> some View
```
As *width* and *float* are nil by default, if we don't specify them, the resulting height will be the same as the content of the view. In the case of alignment, it is often not appreciated, since normally the size of the frame is adjusted to that of the view.

For example, if we want to draw a 200x100 rectangle in red:

```swift
Rectangle()
    .fill(Color.red)
    .frame(width: 200, height: 100)
```
# .offset(_:)
*.offset* is a modifier that allows us to move a view relative to its original position. For example, the red rectangle in the previous example appears in the center of the screen. If we apply an offset(x: 100, y: 100), it will move 100 points to the right and 100 points down.

```swift
Rectangle()
    .fill(Color.red)
    .frame(width: 200, height: 100)
    .offset(x: 100, y: 100)
```
## .onTapGesture(_:)
The *.onTapGesture* modifier allows you to add a gesture to the view, such as a tap gesture. For example:

```swift
Rectangle()
    .fill(Color.red)
    .frame(width: 200, height: 100)
    .onTapGesture {
        print("Hi!")
    }
```
Every time we touch the red rectangle, the action *print("Hi!")* will be executed.



# .opacity(_:)
The *.opacity* modifier assigns transparency to a view. It is assigned a value from 0 (fully transparent) to 1 (opaque).
So, if we use the following code:

```swift
ZStack {
    Text("Hello, world!")
    Rectangle()
        .fill(Color.red)
        .frame(width: 200, height: 100)
        .opacity(0.5)
}
```
Although the rectangle is on top of the text, the text will show slightly since we have assigned an opacity of 0.5 to the rectangle.

## .overlay(_:)
The *.overlay* modifier allows you to add display of a view on top of the parent view (we could consider it the inverse of the *.background* modifier).

For example, if we take the code that comes out when a project is created and we modify it a bit (we make the image in the overlay with a gray background and aligned to the left).

```swift
VStack {
    Text("Hello, world!")
}.overlay(alignment: .leading) {
    Image(systemName: "globe")
        .imageScale(.large)
        .foregroundColor(.accentColor)
        .background(.gray)
}
.padding()
```
What we see is how the icon is above the text and aligned to its left.

# .padding(_:)

With *.padding* we can indicate the space between a view and the border of the view that contains it. That is, it allows us to add a margin around the view. For example:
```swift
VStack {
    Text("Hello, world!").padding(10)
}.background(.red)
```
This code will show us the text 'Hello, world!' inside a red rectangle, but with a space between the text and the borders of the box of 10 points.
We can also specify if we want the margin, for example, to only be on one side:
```swift
Text("Hello, world!").padding(.leading, 10)
```

Or on one axis:
```swift
Text("Hello, world!").padding(.vertical, 10)
```
Or specify a different one for each side:
```swift
Text("Hello, world!").padding(.init(top: 10, leading: 15, bottom: 5, trailing: 50))
```

# .rotationEffect(_:)

We can rotate a view using the *.rotationEffect* modifier. For example, we can flip the text 'Hello, world!' as follows:
```swift
Text("Hello, world!")
            .rotationEffect(.degrees(-180))
```

The angle of twist can also be expressed in radians:
```swift
Text("Hello, world!")
            .rotationEffect(.radians(.pi))
```

# .scaleEffect(_:)
*.scaleEffect* allows us to resize a view, either by making it bigger or smaller.
For example, to multiply the size of *'Hello, world!'* by 2.5:
```swift
Text("Hello, world!")
    .scaleEffect(2.5)
```
The scaling can be different depending on the axis:
```swift
Text("Hello, world!")
    .scaleEffect(x: 2.5, y:10)
```
And we can also scale it and indicate the point (*anchor*) from which the scaling is applied:
```swift
Text("Hello, world!")
    .scaleEffect(x: 2.5, y: 10, anchor: .topLeading)
```
In this case, we scale the text from the top left.
{{<ads2>}}

# Modifiers order
When we are going to apply different modifiers to the same view, we must take into account in which order we apply them. If we think that each time we apply a modifier it is as if we were creating a new view, we will apply (from top to bottom) each modifier to the view that is generated with the previous modifier.

For example:

```swift
Text("Hello, world!")
    .background(.red)
    .frame(width: 200, height: 100)
```

Displays the text 'Hello, world!' with a red background (but only what the text occupies). This is because the frame modifier was applied after the color was applied to the text.
However, if we first apply the frame modifier:

```swift
Text("Hello, world!")
    .frame(width: 200, height: 100)
    .background(.red)
```
What we see is a 200x100 red box in the center of which is the text 'Hello, world!'. This is because we have first set the size of the box, and then we have assigned the background color to that new box.
