---
title: "Xcode 14: One Icon to Rule Them All!"
description: "With Xcode 14 it is no longer necessary to use an application icon for each of the required sizes (depending on where the icon is to be displayed), only one is required. Let's see how to do it."
date: 2022-12-02
categories: ["Design", "Swift"]
tags: ["Development"]
type: "regular" # available types: [featured/regular]
draft: false
---

How many times have we found ourselves preparing countless icons of all the sizes required by Apple for our applications: sizes for iPhone and iPad and for different utilities — Spotlight, Notifications or App.
Generating so many icons of different sizes was tedious if you did it manually, and something simpler and you used an icon generator (which from an icon, normally 1024x1024px, generated all the necessary sizes).
But now with Xcode 14, as Apple announced at WWDC 2022, it will only be necessary to configure the Assets catalog of our application so that it only uses a 1024x1024px icon. Xcode 14 will take care of resizing this icon according to its needs.
{{<ads1>}}

# How to configure an application with a single icon

Once we have created our project with Xcode 14, we go to the Assets catalog. From the outset, we will see the same configuration as before, that is, a template to fill with the different sizes of icons.
{{< image src="images/posts/xcode_14_one_icon_1.png" alt="Xcode 14 one icon">}}

Therefore, we have the option of filling them one by one with the different icons.

{{< image src="images/posts/xcode_14_one_icon_2.png" alt="Xcode 14 one icon">}}

AppIcon asset filled with icons of different sizes.
But, Xcode 14 introduces the option to use a single 1024x1024px icon. To do this, we simply go to the column on the right (to the Attributes inspector) and, in Device, select Single Size (for example, in the case of iOS).

{{< image src="images/posts/xcode_14_one_icon_3.png" alt="Xcode 14 one icon">}}

In this way, the icon template is reduced to a single case, that of 1024x1024px.

{{< image src="images/posts/xcode_14_one_icon_4.png" alt="Xcode 14 one icon">}}

## Limitations
This new functionality of Xcode 14 has some limitations:
* It is only available for iOS, iPadOS, and watchOS. For macOS, icons of different sizes are still required.
* When this option is selected, it is for all icon sizes. That is, if we want an icon of a certain size to be different, we will have to configure them all one by one.
{{<ads2>}}

# Conclusion
Being able to use a single icon for the results of our application both in a reduction in the work time dedicated to generating the icons and in the fact that, by using a single image, the size of our apps is reduced.
