---
title: "9 Tips to Avoid Massive View Controllers in Swift"
description: "Learn how to avoid Massive View Controllers in Swift apps with these 9 tips for keeping your code well-organized, maintainable, and scalable."
date: 2020-01-23
categories: ["Architecture", "Swift"]
tags: ["Development"]
type: "regular" # available types: [featured/regular]
draft: false
---

The **[Model-View-Controller](https://raulferrer.dev/blog/architecture_patterns_ios/)** (**MVC**) design pattern is a basic component of creating app user interfaces, as any iOS developer is aware. However, when an app becomes more complicated, it's not unusual for view controllers to get huge and cumbersome. This can make the code difficult to comprehend and maintain and cause a number of issues, including lengthy build times and poor performance.
{{<ads1>}}

# Model-View-Controller (MVC) components
First of all, let's remember which components make up the Model-View-Controller pattern and what their functions are.

## Model
The **Model** holds de business logic and is in charge of accessing, manipulating, or storing the data of the application:
* Contains classes related to data persistence, like those which use databases (Core Data, SQLite, Realm...) or user preferences (UserDefaults).
* Contains the classes that handles communications (Networking) and that allows the application to receive and send data.
* Contains the classes that are in charge of parse the information received into the application (and convert it into model objects).
* Contains extensions, constants, helper classes...
* A model object can communicate with other model objects with 1:1 and 1:n relationships.
* Direct communication between the Model and the View is not permitted. The Controller serves as the conduit for information exchange between the Model and the View.

## View
The **View** is composed by those components that the user can see:
* These classes are those that come from the UIKit, AppKit, Core Animation, and Core Graphics libraries.
* Despite not being directly related to the Model, they display the data that originates from it (they do it through the Controller).
* Users may interact with these elements.
## Controller
The **Controller** serves as a liaison between the **Model** and the **View**:
* It is the central element of the MVC model and interacts with both the Model and the View.
* It receives and analyzes the user's actions on the View, then updates the Model as necessary.
* If the model's data changes, the view is updated to reflect those changes.
* It looks after the application's life cycle.

# Tips to avoid Massive View Controllers
We'll look at various tactics for avoiding large view controllers in MVC-based programs in this post. Your codebase will be easier to maintain and scale if you use these strategies to keep your view controllers compact, tidy, and understandable.

## 1. Set distinct classes for view controller and logic
The fact that view controllers frequently end up managing a broad range of duties unrelated to their primary function is one common cause of their size. A view controller, for instance, might be in charge of networking duties, data formatting, or business logic.

Consider separating the functionality that is not directly connected to the view controller's primary duties into different classes to prevent this. You might develop a manager class to manage networking operations or a helper class to format data, for instance. By doing so, you'll be able to divide up the functionality into smaller, simpler-to-manage chunks, which will also make the code easier to comprehend and maintain.

## 2. Implement child view controllers
Consider using child view controllers if your view controller contains several sections, each of which has a distinct set of duties. By doing so, you'll be able to divide up the functionality into smaller, simpler-to-manage chunks, which will also make the code easier to comprehend and maintain.

You must first establish a container view in your main view controller before adding the child view controllers as children of the main view controller in order to use child view controllers. The content of the child view controllers can then be shown using the container view.

## 3. Employ delegation
Consider transferring the delegate methods to a different class if your view controller manages a lot of delegate methods. By doing so, you can make your view controller smaller and simpler to understand.

Define a protocol with the delegate methods you want to utilize before having your view controller adopt it in order to use delegation. Then, you may make a different class to serve as the delegate and provide a particular instance of it to your view controller as required.

## 4. Utilize notifications
Consider transferring the notification handling logic to a different class if your view controller is handling a lot of alerts. By doing so, you can make your view controller smaller and simpler to understand.

Create a notification observer in your view controller before registering for the alerts you want to receive in order to use notifications. When necessary, you can send a class instance to your view controller and construct a separate class to handle the notification events.


## 5. Use model classes
Consider developing model classes to manage the data if your view controller is in charge of processing a lot of data. Making your code more understandable will be made possible by separating the view controller from the data management logic.

Model classes should be made to be independent of the view controller and should provide the attributes and methods required to manage the data. This will increase the modularity and maintainability of your codebase and make it simpler to reuse the model classes in other areas of your application.

## 6. Implement protocols and protocol extensions
In Swift, protocols are a useful tool for outlining a class's anticipated behavior. You can describe a collection of methods and attributes that a class must implement using protocols without mentioning the specific implementation. You can write more modular, reusable code as a result.

A protocol's methods can have default implementations by using protocol extensions. If you have several view controllers that must implement the same set of methods, this can be extremely helpful. You can avoid repeatedly writing the same code in each view controller by using protocol extensions.

## 7. Employ custom view classes
Consider putting a lot of the view-related code in your view controller onto a separate custom view class. By doing so, you can make your view controller smaller and simpler to understand.

It is simpler to reuse and maintain view-related code when it is encapsulated in a single location using custom view classes. To further style and modify the user interface of your app, you can add new interface elements like buttons or text fields using custom view classes.

## 8. Use dependency injection
By using the dependency injection technique, objects (such services or helpers) can be passed into a class as dependencies rather than being created there. This can make your view controller smaller and simpler, which will make testing easier.

Define a protocol for the dependency you wish to inject, and then send an instance of that dependency to your view controller when it is initialized if you want to use dependency injection. This will make it simple for you to switch between several dependency implementations as necessary and will facilitate isolation testing for your view controller.

## 9. Utilize functional programming methods
*Map*, *filter*, and *reduce* are examples of [functional programming techniques](https://raulferrer.dev/blog/swift_high_order_functions/) that can be used to build more terse and expressive code. These methods enable declarative manipulation of data sets, which can facilitate code comprehension and maintenance.

If you want to make your view controller logic simpler and clearer, think about adopting functional programming techniques. For instance, you could use filter to choose a subset of data based on specific criteria or map to change the format of an array of data.
{{<ads2>}}

# Conclusion
The MVC design pattern in Swift allows you to avoid huge view controllers using a variety of techniques. Your codebase will be easier to maintain and scale by keeping your view controllers compact, well-organized, and understandable if you adhere to best practices like isolating logic into distinct classes, employing child view controllers, and using functional programming approaches.