---
title: "New architectures for iOS Apps: The ELM Architecture and The Composable Architecture"
description: "The Elm Architecture and The Composable Architecture are two architectures for developing iOS applications that are designed to be easy to reason about and maintain."
date: 2020-01-14
categories: ["Architecture", "Swift"]
tags: ["Development"]
type: "regular" # available types: [featured/regular]
draft: false
---
Surely you have applied or, at least, you know architectures such as MVC, MVVM, MVP (even others such as VIPER or VIP). However, there are also other architectures that have been developed and that are somewhat less known.
For example, in a previous [post I already talked about Redux](https://raulferrer.dev/blog/redux_pattern/) as an architecture that, although it is well known in web development, is perhaps less so in iOS development. But there are others that are perhaps less well known, such as The Composable Architecture (TCA) or The Elm Architecture, of which we will now make a short introduction so that you know them.

## The Elm Architecture (TEA)
The Elm Architecture originated from the use of Elm and the development of webapps. This architecture is based on a Model (which contains the state of the application), a view that is generated according to the model, and an Update that transforms the model.
{{< image src="images/posts/ios_architecture_states_3.png" alt="The Elm Architecture is Swift schema">}}

So, first the Model is passed to the View, which renders it. When the user interacts with the View, this interaction is transmitted in the form of a message to the Runtime (which is sent to the Update together with the current Model).
The Update is responsible for updating the Model according to the information contained in the Message and for developing it at Runtime. The loop restarts when the Runtime returns the updated Model to the View.
## TEA components

Model, Runtime, Update and View are the compoenents of TEA.

* **Model.** Contains the data and the state of the aplication. The model type of the data is defined by typealias. Let's say, for illustration, that we have a component in our application where the model data is the name and age:

```swift
tyepalias Model = User

struct User {
    var name: String
    var age: Int
}
```
* **Runtime.** Relates Model, View and Update.

* **Update.** Is a function that, in response to the Message it receives, modifies the Model (or state). For example:

```swift
class func update(_ message: Message, model: Model) -> Model {
    // Updates the model based on the message
    // (the user action) it has received.
    // For example, change the user name.
    return updatedModel       
}
```
* **View.** Depending on the Model (or state) it receives, returns or renders a view:

```swift
class func view(_model: Model) -> UIView {
    // Updates the View based on the Model
    // (updated) it has been received.
    // For example, a UILabel with the updated user name.
    return updatedView
}
```
### Pros and Cons of TEA

Pros of TEA:
* It has a straightforward architecture and enables declarative view development.
* We can test each component and its behavior with ease because of its logical definition.
* One-way data flow is employed.

Cons of TEA:
* It is a simple architecture, but it can initially be difficult for newbies.
* Furthermore, there is not much literature on the subject.



## The Composable Architecture (TCA)
TCA was developed by [Brandon Williams and Stephen Celis of Point-Free](https://www.pointfree.co/).
This architecture was born to solve some of the problems that we find when developing our applications, like split larger functionalities into small ones, manage the state of the application in a simple way or facilitate the testing of the aplication, unit tests to end-to-end tests.
TCA works in a declarative way, according to reactive programming principles and following Redux patterns. This makes it very suitable for working on iOS projects based on SwiftUI (declarative) although it can also be used with UIKit (imperative).
TCA starts from TEA, but works assuming that each View has its own Store, and each one of the components or children of said View saves a part of that Store. In this way, each time a View generates an Action, it is passed to the Stores of the Parents.
{{< image src="images/posts/ios_architecture_states_1.png" alt="The Composable Architecture is Swift Store propagation">}}

### TCA components
TCA is based on six components: Action, Environment, Reducer, State, Store and View.

* **Action.** In a similar way to how it was described in a [previous Redux post](https://raulferrer.dev/blog/redux_pattern/), we can set an Action as an enum that contains the different actions that can be given. For example:


```swift
enum AppAction {
    case changeUserName(String)
    case changeUserAge(Int)
    case changeUserAddress(String)
}
```
* **Environment.** It includes the dependencies of an application (such as the calls to a server) and the logic of the interaction with the outside (it would be like the Middleware in Redux).
* **Reducer.** Similar to Redux, a Reducer is responsible for taking an Action and the current State to generate a new State. When an Action requires an asynchronous call, the Effects intervene. When an Effect finishes its work, a new Action is produced from the Reducer.

```swift
let reducer = Reducer<State, Action, Environment>  { state, action, environment in
    switch action {
    case .changeUserName(let name):
        state.user.name = name
        return .none
    case changeUserAge(let age):
        state.user.age = age
        return .none
    case changeUserAddress(let address):
        state.user.address = address
        return .none
    }
}
```
* **State.** It represents the state of the application through a set of parameters required by the View.

```swift
struct AppState: Equatable {
    var user: User

    init(user: User) {
        self.user = user
    }
}
```
* **Store.** It is the set formed by State, Action and Reducer. It is in charge of receiving user actions and sending them to the Reducer (together with the State). The newly generated state is passed to the View for it to update.
* **View.** It is in charge of representing the data it receives from the State.

{{< image src="images/posts/ios_architecture_states_2.png" alt="The Composable Architecture is Swift schema">}}


### Pros and Cons of TCA

The pros of TCA are:

* There is a one-way data flow.

* All of the dependencies are located in the environment (this facilitates, for example, change form development to production environments).

* Testing is easier (only Reducers can change the State).

* The application is a composition of diferent features (this allow to design, develop ant test each one independently).

The cons of TCA are:

* UIKit (reactive UI) is not its primary target audience; SwiftUI (declarative UI) is.

* Synchronization can be challenging when there are multiple states.

* Could be difficult for newbiews.
