---
title: "SwiftUI #6. Using the Image component"
description: "Image component is a powerful and flexible way to display images in your app. You can use it to display images from your app bundle, from a remote URL, or from the user's photo library."
date: 2023-01-01
categories: ["SwiftUI"]
tags: ["Development", "Code"]
image: "https://drive.google.com/uc?id=10CbwH4wwiM_kX-FasTbvdtOc0RF0i5H7"
type: "regular" # available types: [featured/regular]
draft: false
---

{{< youtube QBlLpGJ846s >}}

The **Image** component in SwiftUI is a potent and adaptable way to show images in your app. It can be used to show pictures from the user's photo collection, a remote URL, or your software bundle. Modifiers allow you to adapt the appearance and behavior of the Image component to your own requirements.
{{<ads1>}}

# Image source
## Bundle
Here's an illustration of how you can display a picture from your app bundle using the Image component (first we must add this image in the *Assets* folder of our project):

```swift
struct ContentView: View {
    var body: some View {
        Image("exampleImage")
    }
}
```

## Internet

To load images from internet, ywe need the **AsyncImage** component, which accepts a URL argument, to display an image from a remote URL:

```swift
struct ContentView: View {
    var body: some View {
        AsyncImage(url: URL(string: "https://images.freeimages.com/images/large-previews/218/my-dog-cutter-1499799.jpg"))
    }
}
```

## PhotosPicker (iOS16)
To display an image from the user's photo library, we can use the PhotosPicker component:

```swift
import SwiftUI
import PhotosUI

struct ContentView: View {
    @State var selectedItems: [PhotosPickerItem] = []
    var body: some View {
        PhotosPicker(
            selection: $selectedItems,
            matching: .images,
            photoLibrary: .shared()) {
                Text("Choose an image!")
            }
    }
}
```

# Modifiers
## .resizable()
You can specify how the picture should be scaled to fit its parent container using the *resizable()* modifier. A set size, proportional size, or a minimum and maximum size can all be specified.

Here is an illustration of how to fit an image to its parent container with a fixed size using the *resizable()* modifier:

```swift
struct ContentView: View {
    var body: some View {
        Image("exampleImage")
            .resizable()
            .frame(width: 100, height: 100)
    }
}
```
In this way we adjust the size of the image to the measurements that we have given it, which can cause the image to be deformed if its proportion is not the same as that of the frame that contains it.


## .scaledToFit() and scaledToFill()
If we want to display the image without deforming it, we must add a new modifier: *.scaleToFit()* or *scaleToFill()*.
The function of *.scaleToFit* is to scale the image, maintaining the proportions, until one of the two measurements (width or height) fits the measurement of the corresponding axis of the frame.
For example, a very narrow image will fit the height of the frame (Y axis), but there will be white space on the sides of the image (X axis).

```swift
struct ContentView: View {
    var body: some View {
        Image("exampleImage")
            .resizable()
            .scaledToFit()
            .frame(width: 300, height: 300)
    }
}
```

On the other hand, *.scaleToFill* scales the image until the entire frame is covered by the image, but maintaining the proportions, even if the image is not displayed in its entirety later. For example, in the case of the elongated image, it would be enlarged until the width of the image was the same as that of the frame, but by maintaining the proportions, the height would be much greater and would be outside the frame (the image would look cut).

```swift
struct ContentView: View {
    var body: some View {
        Image("exampleImage")
            .resizable()
            .scaledToFill()
            .frame(width: 300, height: 300)
    }
}
```
The effect of these two modifiers is the same as using the *.aspectRatio(_:contentMode:)* modifier, to which we can assign the values *.fit* or *.fill*.
```swift
struct ContentView: View {
    var body: some View {
        Image("exampleImage")
            .resizable()
            .aspectRatio(contentMode: .fit)
            .frame(width: 300, height: 300)
    }
}

struct ContentView: View {
    var body: some View {
        Image("exampleImage")
            .resizable()
            .aspectRatio(contentMode: .fill)
            .frame(width: 300, height: 300)
    }
}
```

## .clipShape()
The image can be clipped to a certain shape using the *.clipShape()* modifier. You can pick a pre-existing form like *Circle*, *Capsule*, or *Rectangle*, or you can use a *Path* to design a unique shape.

Here is an illustration of how to clip the image to a circle using the *.clipShape()* modifier:

```swift
struct ContentView: View {
    var body: some View {
        Image("exampleImage")
            .clipShape(Circle())
    }
}
```

Using a *Path*:

```swift
struct ContentView: View {
    var body: some View {
        Image("exampleImage")
            .clipShape(
                Path { path in
                    path.move(to: CGPoint(x: 0, y: 0))
                    path.addLine(to: CGPoint(x: 100, y: 0))
                    path.addLine(to: CGPoint(x: 75, y: 100))
                    path.closeSubpath()
                }
            )
    }
}
```
{{<ads2>}}

## .overlay()
You can overlay a view on top of the image with the *.overlay()* modifier. Adding text, buttons, or other interactive elements is a good usage for this.

Here is an illustration of how to put a button to the top of the image using the *.overlay()* modifier:

```swift
struct ContentView: View {
    var body: some View {
        Image("exampleImage")
            .overlay(
                Button(action: {
                    // button action
                }) {
                    Text("Button")
                }
    }
}
```

## .shadow()
You can give the image a shadow by using the shadow() modifier. The hue, radius, and offset of the shadow are all programmable.

Here's an illustration of how to add a shadow to the image using the *.shadow()* modifier:

```swift
struct ContentView: View {
    var body: some View {
        Image("exampleImage")
            .shadow(color: .black, radius: 10, x: 5, y: 5)
    }
}
```

## .blur()
You can blur the image by using the *.blur()* modifier. The blur mode and radius are also programmable.

Here is an illustration of how to blur an image using the *.blur()* modifier:

```swift
struct ContentView: View {
    var body: some View {
        Image("exampleImage")
            .blur(radius: 10)
    }
}
```

And it's possible to specify the blur mode using the BlurEffect.Style enum:

```swift
struct ContentView: View {
    var body: some View {
        Image("exampleImage")
            .blur(radius: 10, opaque: false)
    }
}
```
