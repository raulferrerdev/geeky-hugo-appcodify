---
title: "MVVM Architecture with RxSwift"
description: "Learn how to use RxSwift, a potent tool for dealing with asynchronous events and updating the UI in a declarative and modular manner, to put the MVVM architecture into practice."
date: 2018-07-22
categories: ["Architecture", "Swift"]
tags: ["Development"]
type: "regular" # available types: [featured/regular]
draft: false
---

**MVVM (Model-View-ViewModel)** is an [architecture pattern](https://raulferrer.dev/architecture_patterns_ios/) that wants to separate the presentation logic from the business logic. It's composed by a **Model** layer, a **View** layer and a **ViewModel** layer.

### MVVM Structure

First, let's make a small introduction for **MVVM architecture**.

### Model
Contains the business logic and it's also in charge of deal with data (storing, accesing and manipulating data). The **Model** can only communicate with the **ViewModel** layer (so is unaware of the existence of the **View**).

### View
This layer contains the **View** (both the UIView and the UIViewController classes). This layer is in charge of show information to the user but has no logic, and uptading comes from the **ViewModel** (the View can have multiple references to the **ViewModel**).
In the case of the UIViewController, it's only in charge of navigation between views (at most, pass information, using a Delegate pattern).

### ViewModel
This layer is stands between the View and the Model, and is the central component of the **MVVM architecture**, and processes the input/output data that controls the view. 

## Data flow in MVVM

In the **MVVM architecture** pattern, the **View** is linked to the **Model** through the **ViewModel** thanks to a concept called **Data Binding**. 

**Data binding** is a means to automatically link the View to the data in the Model. With **data binding**, the manually updating of the UI with new information is avoiding as we work with a reactive code.

There are some methods to do Data Binding (with 3rd party frameworks like RxSwift, with [KVO](https://raulferrer.dev/kvc_and_kvo_swift/) or manually, doing our own code). In this post we will use [RxSwift](https://raulferrer.dev/rxswift_introduction/). 

[RxSwift](https://github.com/ReactiveX/RxSwift) is a well-known library that adds **reactive programming** to Swift. It allows developers to handle asynchronous events declaratively and modularly, making it ideal for implementing the MVVM pattern.

## Let's write some code

### Model
Let's start our example by defining the **Model**. In this case, it will be a simple **Model** for a *User* (with *name* and *age* parameters):

```swift
struct User {
    let name: String
    let age: Int
}
```

### ViewModel
Then, we build the **ViewModel**. As we have seen, the **ViewModel** interacts with the **Model** and with the **View**. What we will do is something simple, **ViewModel** will take the name and will capitalize it.
Using MVVM with RxSwift, I prefer to use a [Input/Output convention](https://github.com/kickstarter/ios-oss):
•** Input.** It refers to all of the events and interactions that take place in the **View** and have an impact on the **ViewModel**.
• **Output.** These are the modifications to the **Model** that must be reflected in the **View**.
```swift
import RxSwift
import RxCocoa

class ViewModel {
    
    var output: Output!
    var input: Input!
    private let disposeBag = DisposeBag()

    struct Input {
        let user: BehaviorRelay<User>
    }
    
    struct Output {
        let title: Driver<String>
    }
        
    init() {
        let user = BehaviorRelay<User>(value: User(name: "", age: 0))
        
        let capsTitle = user
            .map({ $0.name.uppercased() })
            .asDriver(onErrorJustReturn: "")
        
        input = Input(user: user)
        output = Output(title: capsTitle)
    }
}
```

The **ViewModel** class contains two inner classes, *Input* and *Output*. The inputs that the **ViewModel** accepts from the **View** layer are defined by *Input*, and the outputs that the **ViewModel** provides to the **View** layer are defined by *Output*.

The *Input* class in this example has a single property called *user*, which is a *BehaviorRelay* that holds a *User* object. The *BehaviorRelay* RxCocoa Subject has a default initial value and emits the most recent value to its subscribers. This property will be used to update the **ViewModel** with the user's information.

The *Output* class only has one property, *title*, which is a *Driver* that emits a String value. Another type of RxCocoa Subject that is used to handle UI bindings is the *Driver*. It is a type of *Observable* that is guaranteed to always emit events on the main thread.

We created a *BehaviorRelay* object in the *init()* method of the // class to hold the user's information. The *map()* operator was then used to convert the *User* object to a String by capitalizing the user's name. Finally, we used the *asDriver()* operator to convert the *Observable<String>* into a *Driver<String>*, which we assigned to the *Output* object's *title* property.

### View
Let's now create the View, which will only displays the user's name:

```swift
import UIKit
import RxSwift
import RxCocoa

class View: UIViewController {
    
    private let disposeBag = DisposeBag()
    var viewModel: ViewModel!
    var nameLabel: UILabel = UILabel()
    var ageLabel: UILabel = UILabel()

    override func viewDidLoad() {
        super.viewDidLoad()
        configureUI()
        rxSetting()
    }

    func configureUI() {
        ...
    }

    func rxSetting() {
        // Bind the ViewModel's outputs to the View's inputs
        viewModel.output.title
            .drive(nameLabel.rx.text)
            .disposed(by: disposeBag)
        
        // Bind the View's inputs to the ViewModel's inputs
        nameLabel.rx.text
            .orEmpty
            .bind(to: viewModel.input.user.map({ $0.name }))
            .disposed(by: disposeBag)
    }
}
```
This view contains two UILabels that show the user's name and age. Also, a disposeBag property was made to hold subscriptions.

We have bind the *title* output of the **ViewModel** to the text input of the *nameLabel* with a *drive()* operator. We have also used a *bind()* operator to connect the text input from the *nameLabel* to the *user* input from the **ViewModel** (the *orEmpty* operator allows to do the bind although the text value is nil).

At he end, we can link the View with the ViewModel:

```swift
let viewModel = ViewModel()
let view = View()

view.viewModel = viewModel
```

## Conclusion

In conclusion, using RxSwift to implement the **MVVM architecture pattern** offers a reliable and scalable method for creating iOS apps. We can build a more maintainable codebase and test each component independently by separating the roles of the **Model**, **View**, and **ViewModel**. We can update the UI and handle asynchronous events declaratively and modularly by using RxSwift, which produces code that is cleaner and shorter. We can develop iOS apps that are adaptable and simple to maintain by utilizing RxSwift and the **MVVM architecture pattern**.

