---
title: "How to add Dark Mode in iOS 13 (set iPhone background black)"
description: "Find out how to add Dark mode to your iOS apps. Learn to adapt them and use adaptive colors."
date: 2022-12-04
categories: ["Swift", "Design"]
tags: ["Development"]
type: "regular" # available types: [featured/regular]
draft: false
---

## Set iPhone background black and keep visibility
Surely you have heard of **Dark Mode** on iOS 13, that is, a mode in which the color range of the user interface darkens. This allows, among other things, to improve visibility in places with low light and reduce the energy consumption of applications (dark colors use less energy than light ones). For example, we can set our iPhone background black (as using dark mode), and then, the foreground color will change to white.

Although some applications already included a Dark Mode in their interface, it was not until the publication of iOS 13 and iPadOS, that the Dark Mode was included in the configuration of the device itself.
{{<ads1>}}

## Adapt applications to Dark Mode

To adapt an application to Dark Mode we must work with Xcode 11 and iOS 13. In addition, we must modify the colors and images used in the application.

### Use adaptive colors (create dark mode color palette)

Colors that adapt automatically to the style of the interface should be used. For this we have two possibilities:

- Use semantic colors. It is a set of colors prepared by [Apple](https://developer.apple.com/design/human-interface-guidelines/ios/visual-design/color/), which have two variants: Dark Mode and Light Mode.

{{< image src="images/posts/add_dark_mode_ios_1.png" alt="Set Dark mode on iOS">}}

To use these colors, for example, from the code, just write:
```swift
 let color = UIColor.systemGreen
```

- Or if we are in the Interface Builder, select it in the color attribute of the component we want to color.

### Define custom colors

As we saw in [How to create a color palette (Color Assets) in Xcode](https://raulferrer.dev/blog/create_color_palette/), we can define our own color palette in Xcode, so that we can call from any point of the application to these colors (both from the code and from the Interface Builder)

To do this, we select the Assets.xcassets folder (or creating our own .xcassets folder for colors) in the project navigator (Project Navigator).

Then we right-click, select New Color Set and change its attributes to generate the new color.
{{<ads2>}}

### Two versions of the same color

But now we must generate two versions of the same color: the version of the light or default mode and that of the dark mode.

To do this, from the Color Attributes Inspector we have created, we select the Apperances attribute, and in the Any, Dark option.

{{< image src="images/posts/add_dark_mode_ios_2.png" alt="Color Attributes Inspector">}}

When doing this the color appears to us but with two versions: Any Appearance and Dark Appearance. From the property inspector we indicate the color for each of them.

{{< image src="images/posts/add_dark_mode_ios_3.png" alt="Setting Appearance and Dark Appearance colors">}}

In this way, when the device mode is changed, our application will automatically take the color that corresponds to it.

{{< image src="images/posts/add_dark_mode_ios_4.png" alt="Setting Appearance and Dark Appearance colors">}}

To be able to change the mode in the simulator, once the application is launched on it, we select the icon that represents two switches in the lower Xcode bar, and then indicate the interface style that we want to be applied.

{{< image src="images/posts/add_dark_mode_ios_5.png" alt="Example of Appearance and Dark Appearance colors">}}
