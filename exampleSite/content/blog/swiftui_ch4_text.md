---
title: "SwiftUI #4. Using the Text component"
description: "Text is a SwiftUI component that allows us to display text on the screen. The different modifiers that we can apply to this component allow us to greatly customize it."
date: 2022-12-24
categories: ["SwiftUI"]
tags: ["Development", "Code"]
image: "https://drive.google.com/uc?id=1Fa0oMOIq8WE0_JKeSCADy8_ijODiJ0TV"
type: "regular" # available types: [featured/regular]
draft: false
---

{{< youtube ev0Bux_NSKU >}}
SwiftUI's core component, **Text**, is used to display and input text in our apps. It is a view from the SwiftUI framework that symbolizes a line of text.
{{<ads1>}}

A string of text can be displayed in our user interface using SwiftUI's **Text** view. By providing a string as the argument to its initializer, we may construct a Text view:

```swift
Text("Hello, World!")
```
**Text** that is kept in our view's String property can also be displayed using the Text view:

```swift
struct ContentView: View {
  let message: String

  var body: some View {
    Text(message)
  }
}
```
The **Text** view includes a number of modifiers that we can use to alter how it looks and behaves in addition to displaying text.
# .font(_:)
We can modify the text's font using this modification. To indicate the preferred font, a Font value can be passed as an input. To use the system's huge title font, for instance, we can use *.largeTitle*:

```swift
Text("Hello, World!")
  .font(.largeTitle)
```
Making use of the initializer *Font.custom(:size:)*, we can also design a unique font. This initializer accepts two arguments, the font's name and the preferred font size:

```swift
let customFont = Font.custom("Avenir", size: 24)

Text("Hello, World!")
  .font(customFont)
```
# .foregroundColor(_:)

We can modify the text's color using this modification. We can indicate the preferred color by passing in a Color value as an input. For instance, to make the text red, we can use the *.red* color:

```swift
Text("Hello, World!")
  .foregroundColor(.red)
```

Using the initializer *Color(red:green:blue:opacity:)*, we can also create a unique color. This initializer requires red, green, and blue channel values in the range of 0 and 1, as well as an opacity value in the range of 0 and 1:

```swift
let customColor = Color(red: 0.5, green: 0.2, blue: 0.9, opacity: 1)

Text("Hello, World!")
  .foregroundColor(customColor)
```
# .lineLimit(_:)

By using this modification, we can restrict the number of text lines that are displayed. A **Text** view always shows the entire text that it contains by default. However, we may supply a value as an argument to this modifier if we wish to restrict the number of lines. For instance, we can use the code below to make the content only two lines long:

```swift
Text("Hello, World!")
  .lineLimit(2)
```

The text will be truncated and an *ellipsis* (...) will be displayed at the end if it has more lines than the number given by this modification.
# .multilineTextAlignment(_:)

With the help of this modification, we may align the text in a multi-line **Text** view. The text is oriented to the left by default. However, we can change the alignment by using this modification. For instance, we may use the following code to center the text:

```swift
Text("Hello, World!")
  .multilineTextAlignment(.center)
```

Additionally, we can use *.leading* to align text to the left, *.trailing* to align text to the right, or *.justified* to justify text.

# .lineSpacing(_:)

We can alter the gap between lines of text using this modification. The lines are uniformly spaced by default. The modifier can, however, be used to specify a different line spacing. For instance, we can use the following code to add 10 points of space between each line of text:

```swift
Text("Hello, World!")
  .lineSpacing(10)
```
{{<ads2>}}

# .truncationMode(_:)
This modifier controls how the text is truncated when it exceeds the bounds of the Text view. By default, the text is truncated at the end and an *ellipsis* (...) is displayed. However, we can use this modifier to specify a different truncation mode. For example, to truncate the text at the beginning and display an *ellipsis* at the start, we can use the following code:

```swift
Text("Hello, World!")
  .truncationMode(.head)
```

We can also use *.middle* to truncate the text in the middle and display ellipses on both ends, or *.tail* to truncate at the end and display an *ellipsis* (the default behavior).
