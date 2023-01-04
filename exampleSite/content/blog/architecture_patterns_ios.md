---
title: "Pros and cons of some iOS Architecture patterns: MVC. MVP, MVVM, VIPER, and VIP"
description: "Some pros and cons of some of the most used architecture patterns in the development of iOS applications: MVC. MVP, MVVM, VIPER, and VIP."
date: 2017-12-01
categories: ["Swift", "Architecture"]
tags: ["Development", "Code"]
type: "regular" # available types: [featured/regular]
draft: false
---
Since the introduction of iOS as an Operating System, the programming of its applications has evolved in terms of the architecture on which the code of said applications was based.
Starting with the MVC (Model-View-Controller) architecture recommended by Apple, other architectures have been used, each one with its pros and cons.
In this post we are going to see some of these pros and cons for five of the most used or most projected architectures today: MVC, MVVM, MVP, VIPER and VIP.

#### 1. MVC: Model-View-ViewController
In this architecture, the application logic is found in the ViewController, which acts as a link between the View and the Model.
##### Pros
* It's simple in design.
* It enables quick development of straightforward apps.
* Compared to other architecture patterns, it uses less code.
* Outlines a distinct division of labor; each element's responsibilities are well-defined.
##### Cons
* The controllers are not very reusable because they are closely related to the View and the Model.
* The Controller is derived from the UIViewController class, where the relationship between the Controller and the View is strong. This means that the separation of responsibilities is typically lost (this difficults thethe Controller on its own).
* There is a strong propensity to "overload" the Controller with duties that are not appropriate for it, such as delegates and the data sources for tables and collections, navigation, and a portion of the business logic. As a result, a "Massive View Controller" is created.

#### 2. MVVM: Model-View-ViewModel
In MVVM, the ViewModel (which contains the business logic) acts as an intermediary between the View (which includes the UIViewController) and the Model.
##### Pros
* The ViewModel is in charge of processing the Model data to display it in the View, so is a clear separation of responsibilities (keeping simple its maintenance).
* It enhances testability since we can test the business logic of the ViewModel without having to take the View into consideration.
* Controllers don't rely on the Model like MVC does, testing them is also simpler.
##### Cons
* In the same way as the Controller in MVC, the ViewModel can also be overloaded with functions.
* The Data binding used in the connection between the View and the ViewModel is usually done with external frameworks, so the application's size and speed will both rise.
* Data binding uses a declarative paradigm, which can be challenging to debug because it instructs the software what to do rather than how to do it (imperative paradigm).

#### 3. MVVP: Model-View-Presenter
In MVP, the Presenter acts as an intermediary between the View (which includes the UIViewController) and the Model, and contains the business logic.
##### Pros
* Compared to the MVC pattern, it offers a greater separation of duties.
* Although it is a little more complex than MVC, it is derived from it, so we can quickly get accustomed to using it.
* Business logic testing could be improved.

##### Cons
* Because it is more complex than MVC, it is not typically advised for usage in small and straightforward applications.
* The Presenter in the MVP has the same potential to grow into a significant component as the Controller did in the MVC.
* Despite the fact that we have further modularized the design, there are still some problems, such as the Controller's continued control over screen switching.

#### 4. VIPER: View-Interactor-Presenter-Entity-Router
VIPER is a more complex architecture with five components with separated responsibilities. For example, the Interactor contains the businness login, the Presenter acts as the center of this architecture, and the router holds the creation and navigation between screens.
##### Pros
* The code is cleaner and decoupled since the Single Responsibility Principle (SRP) is one of its guiding principles.
* Due to this, creating unit tests is easier.
* The code is written with more abstraction (this facilitates add new features and to scale the product).
* VIPER works with separeted layes (as Clean Architecture), so it's simpler to create automated tests.
* VIPER is aimed to work with distinct modules that have clearly defined communication interfaces. This allos projects to be divided among various developers.
* Separation of business logic it's very helpful when large and complicated applications are developed.

##### Cons
* A lot of boilerplate code must be written, and coul be overhealmed for newbies.
* Each module holds so many classes in each module, so each project will have a lot of code and classes.
* Due to its complexity, VIPER it'is not usually recommended for small applications that will not have new features.
* If you usually work with storyboards and segues, keep in mind that its use is not recommended, since it reduces the reusability of the modules.

### 5.VIP: View-Interactor-Presenter
VIP, unlike the other architectures discussed, works with a one-way flow of information between the View, the Interactor and the Presenter.
##### Pros
* Follows Clean Architecture's principles.
* The flow of data is unidirectional.
* Uses Simple Responsibility Principle, which results in smaller methods.
* Uses protocols (interfaces) in order to make code more modular and reusable (so we can change one piece for another without impact on the rest of the application).
* It's easy to maintain and debug.
* Unit tests can be easily added.

##### Cons
* VIP have a lot of protocols and boilerplate code, which can be confusing. It's advisable to use templates.
* For newbies could be overhelming, as at first sight seems over-engineered.
* As VIPER, is not usually recommended in small applications.
* It must be taken into account that, although it is a small functionality to add, the amount of code to write is high.
#### Conclusion
As you can see, each of the architectures you have seen has its pros and cons, so it's not possible to talk about one architecture being better than the other: you just have to know how to apply them in each case.
