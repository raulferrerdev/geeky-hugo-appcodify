---
title: "How to create a color schema (Color Assets) in Xcode"
description: "Learn how to create a color schema in Xcode and how to define all the colors in an application in a single point."
date: 2020-01-04
categories: ["Swift", "Design"]
tags: ["Development", "Code"]
type: "regular" # available types: [featured/regular]
draft: false
---

## Creating a Color Schema
The *Asset Catalog* of colors are the **color schema** that we can create, from the publication of iOS 11 and Xcode 9, to have in a single point the definition of all the colors of our applications.

This will allow us, on the one hand, to use these colors both from the code and from the graphical interface (storyboard and xib), and, on the other, prepare our application to use the Dark Mode (available in iOS 13 and Xcode 11), such as shown in [How to add Dark Mode in iOS 13](https://raulferrer.dev/blog/add_dark_mode_ios/).
{{<ads1>}}

## Creation of a color

To create a color for our **color schema**, we can do it directly by selecting the *Assets.xcasset*s folder (or creating our own .xcassets folder for colors) in the project navigator (Project Navigator).

{{< image src="images/posts/create_color_palette_1.png" alt="Xcode color palette">}}


Then we right click and select *New Color Set*. In this way we have created a new color and, if we access the attributes window, we can change its name and its properties. In this case, we will call this new color *greenBackgroundColor*.
{{< image src="images/posts/create_color_palette_2.png" alt="Xcode color palette">}}

## Use of new colors in storyboard and xib files

To use the new color of our **color schema** that we have created from the graphical interface, we simply have to select the properties of the element to which we want to apply the color and select it from the attribute inspector view (*Attributes inspector*). For example, if we create a label (UILabel), we can go to the *Color* property, and there select the color we have created.
{{< image src="images/posts/create_color_palette_3.png" alt="Xcode color palette">}}
{{<ads2>}}


## Use of new colors in the code

To use the new color of our **color schema** created directly in the code, the best way is to create an extension for the UIColor class and then create a variable with the color name.

For example:
```swift
extension UIColor {
  static let greenBackgroundColor: UIColor = UIColor(named: "GreenBackgroundColor")!
}
```

Which allows us to use this new color as follows:
```swift
let labelColor = UIColor.greenBackgroundColor
```
