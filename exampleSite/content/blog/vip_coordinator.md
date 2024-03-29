---
title: "VIP Architecture with Coordinator"
description: "The Clean Swift (VIP) architecture pattern is popular because it divides responisibilities into distinct components, making it easier to manage the codebase as it grows. In this post, we'll show how to use a configurator to implement the Clean Swift (VIP) architecture pattern, as well as provide an example of a to-do list app to demonstrate the pattern in action."
date: 2020-02-12
categories: ["Swift", "Architecture"]
tags: ["Development", "Code"]
image: "https://drive.google.com/uc?id=1-55gtLjg1VB7hNHtaP9MrxDOaBy0UbvU"
draft: false
---

Clean Swift, also known as VIP (View-Interactor-Presenter) architecture, is an iOS application design pattern that [Raymond Law introduced it in 2014](https://clean-swift.com/) as an alternative to the popular Model-View-Controller (MVC) pattern. VIP architecture's primary goal is to make the codebase more modular, testable, and scalable.
{{<ads1>}}

# When should you use VIP architecture?
VIP architecture is well suited for large, complex iOS applications that necessitate the collaboration of multiple developers. It's also useful for applications that need a lot of test coverage and upkeep.

# VIP Components

VIP architecture is made up of five major components:


## View

The View layer is in charge of displaying data to the user as well as handling user interactions. It is a passive layer that is unaware of the underlying business logic.

## Interactor

The Interactor layer contains the application's business logic. It receives requests from the Presenter layer and executes the required operations. Data retrieval, processing, and validation are all handled by the Interactor layer.

## Presenter

The Presenter layer acts as a go-between for the View and the Interactor. It takes user input from the View and sends it to the Interactor for processing. The Presenter layer is also in charge of formatting data from the Interactor and passing it to the View for display.

## Router

The Router layer is in charge of application navigation. Based on user actions and business logic, it determines which screen to display.

## Entities

Entities represent the application's data models. They include the properties and methods necessary to represent business objects. Swift structs or classes are commonly used to implement Entities.


# Using a Configurator

The Configurator component in the VIP architecture pattern is in charge of configuring components for a given view controller. The Configurator function is typically implemented as a separate class or module, with the goal of decoupling the VIP component setup from the view controller itself.

The Configurator creates and configures the view controller's VIP components, such as the Interactor, Presenter, and Router. It also creates the links between the components and the view controller and returns the view controller with the components attached once the components have been configured.

The Configurator allows to encapsulate and segregate the components from the view controller . Additionally, because the VIP components can be tested independently of the view controller, it offers a cleaner, more modular codebase that is simpler to test.

# Example code

Let's see an schematic example of the application of VIP architecture with Configurator. As one of the main drawbacks of the VIP architecture is the boilerplate code need for fill all the classes in a scene, we will [use a template](https://raulferrer.dev/blog/xcode_templates/) (you can find many Xcode templates for VIP over Internet).

## View+ViewController
{{< highlight swift "linenos=inline,linenostart=1" >}}
protocol ExampleViewDelegate: class {}

final class ExampleView: UIView {
    
    weak var delegate: ExampleViewDelegate?

    override init(frame: CGRect) {
        super.init(frame: frame)
        configureUI()
    }
    
    required init?(coder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
    
    func configureUI() {}
}
{{< /highlight >}}


```swift
import UIKit

protocol ExampleViewControllerInput: class {}

protocol ExampleViewControllerOutput: class {}

final class ExampleViewController: UIViewController {
    var interactor: ExampleInteractorInput?
    var router: ExampleRouterDelegate?
    
    private let sceneView: ExampleView
    
    init(sceneView: ExampleView) {
        self.sceneView = sceneView
        super.init(nibName: nil, bundle: nil)
    }
    
    required init?(coder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
    
    override func viewDidLoad() {
        super.viewDidLoad()
        sceneView.delegate = self
        self.view = sceneView
    }
}

extension ExampleViewController: ExampleViewControllerInput {}

extension ExampleViewController: ExampleViewDelegate {}
```

## Interactor
```swift
import Foundation

protocol ExampleInteractorOutput: class { }

typealias ExampleInteractorInput = ExampleViewControllerOutput

final class ExampleInteractor {
    var presenter: ExamplePresenterInput?
}

extension ExampleInteractor: ExampleInteractorInput {}
```

## Presenter
```swift
import UIKit

typealias ExamplePresenterInput = ExampleInteractorOutput
typealias ExamplePresenterOutput = ExampleViewControllerInput

final class ExamplePresenter {
    weak var viewController: ExamplePresenterOutput?
}

extension ExamplePresenter: ExamplePresenterInput {}
```
## Router

```swift
import UIKit

protocol ExampleRouterDelegate {}

final class ExampleRouter {
    weak var viewController: UIViewController?
}

extension ExampleRouter: ExampleRouterDelegate {}
```

## Configurator
```swift
import Foundation

final class ExampleConfigurator {
    
    static func configure( _ viewController: ExampleViewController) -> ExampleViewController {
        
        let interactor = ExampleInteractor()
        let presenter = ExamplePresenter()
        let router = ExampleRouter()
        router.viewController = viewController
        presenter.viewController = viewController
        interactor.presenter = presenter
        viewController.interactor = interactor
        viewController.router = router
        return viewController
    }
}
```

## Configurator code explained

In the ExampleConfigurator, we have a method (configure) that takes an instance of ExampleViewController as an argument and returns an instance of ExampleViewController, which will be 'configured'. Inside this method, this is waht occurs:

* Instances of ExampleInteractor, ExamplePresenter, and ExampleRouter are created.
* The viewController properties of the router and the presenter are set to the viewController passed.
* The presenter property of the interactor instance is set to the presenter instance created at the beginning.
* The interactor and router instances are assigned to the interactor and router properties of the viewController.
* Finnally, the 'configured' viewController passed to the function is returned.
So we have set up all the dependencies required for the ExampleViewController scene to work.

## Protocols nomenclature
As you have seen, there are a lot of protocols that allows the flow of informaction. In this case, we have also use a *Input/Output* convention to define the inputs and outputs of each component. In order to accomplish that, we use *typealias* to 'rename' this protocols in each case.
{{<ads2>}}

# Advantajes and drawbacks
As any other architecture, VIP has advantajes and drawbacks.

## Advantajes
* Follows Clean Architecture’s principles.
* The flow of data is unidirectional.
* Uses Simple Responsibility Principle, which results in smaller methods.
* Uses protocols (interfaces) in order to make code more modular and reusable (so we can change one piece for another without impact on the rest of the application).
* It’s easy to maintain and debug.
* Unit tests can be easily added.

## Drawbacks
* VIP have a lot of protocols and boilerplate code, which can be confusing. It’s advisable to use templates.
* For newbies could be overhelming, as at first sight seems over-engineered.
* Is not usually recommended in small applications.
* It must be taken into account that, although it is a small functionality to add, the amount of code to write is high.