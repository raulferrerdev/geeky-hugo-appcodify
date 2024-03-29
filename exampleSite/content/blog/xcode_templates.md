---
title: "Xcode templates: Reduce the development time of your apps"
description: "Xcode templates help developers save time by providing pre-written code for common features, reducing repetitive tasks and speeding up development. Use of Xcode templates can improve productivity, reduce errors and lead to better code quality."
date: 2018-04-21
categories: ["Swift", "Architecture"]
tags: ["Development", "Code"]
type: "regular" # available types: [featured/regular]
draft: false
---

A few months ago I wrote a post about the advantages and disadvantages of some of what I considered to be the [most used architecture patterns](https://raulferrer.dev/blog/architecture_patterns_ios/) in the development of iOS applications (MVC, MVP, MVVM, VIPER and VIP).

When working with each of these patterns in some projects, I realized one of the possible disadvantages that patterns like VIPER or VIP had: the large number of classes and repetitive code that was needed for their operation.

But this is where **Xcode templates** come into play.
{{<ads1>}}

# What are Xcode templates

Every time you create a new file in Xcode, a screen appears in which you have to select the type of file. When you select it, within the new file you will see some lines of code that make it easier for you to start working on that file.

For example, if we create a *Cocoa Touch Class* file and tell it to be a subclass of *UIButton*, this code appears:

```swift
import UIKit

class CustomButton: UIButton {

    /*
    // Only override draw() if you perform custom drawing.
    // An empty implementation adversely affects performance during animation.
    override func draw(_ rect: CGRect) {
        // Drawing code
    }
    */

}
```
That is, there is an import of UIKit, and then it generates the CustomButton class (the name we have given to the file) which is a subclass of UIButton. It also gives us some more indication on how, for example, override of draw method.

In other words, **Xcode templates** are prewritten code that can be inserted into a project to quickly add new features or functionality.

# What are the advantajes of the Xcode templates

Some of the advantages of using Xcode templates are:
* Save time and effort during the development process by reducing repetitive tasks and speeding up the overall process.
* By reducing time on repetitive tasks, Xcode templates can help developers focus on the unique aspects of a project, freeing up time and improving overall productivity.
* The fact that using this preset code can help reduce possible errors when programming.

# How to create our own Xcode template
In addition to using the templates provided by Xcode, we can also create our own. But first, let's see what an Xcode template looks like.

## Structure of an Xcode template
To see what a template is like in Xcode, we access the following folder (it is located inside the Xcode app itself): */Applications/Xcode.app/Contents/Developer/Library/Xcode/Templates/File Templates/Source*

If we now access the folder named *Swift File.xctemplate*, we can see two main files (we can also find a couple of .png files, which are the icons that are shown on the template selection screen):

* **__FILEBASENAME__.swift.** This is the file that contains the prewritten code. When we select a template, Xcode takes care of duplicating this file and giving it the name we have given it (replacing the value of *__FILEBASENAME__*). In this case, the code is as simple as:

```swift
import UIKIt
```

* **TemplateInfo.plist.** This file stores information about the template.

> It must be taken into account that, although Xcode presents its templates in the mentioned folder, when creating our own it is better to do it outside the application, since when updating Xcode we would lose them. To do this, they are created in the folder *~/Library/Developer/Xcode/Templates/File Templates*.

# Let's create our custom templates
We are now going to create our own templates. For this we will take, as mentioned at the beginning, the architecture patterns VIPER and VIP.
These architecture patterns work with numerous classes that communicate with each other through protocols, which makes it advisable to establish templates that already have all this repetitive code inside (and that, in many cases, we customize according to our project).
The idea is to create a template that, just by entering the name of the screen that we want to create, generates all the necessary classes for us.

## VIPER
VIPER is an architecture that divides application functionality into five components: View, Interactor, Presenter, Entity, and Router.
It separates responsibilities within the app, using protocols to communicate between layers and adhering to the Single Responsibility and Interface Segregation Principles.
The View displays information, the Interactor handles business logic, the Presenter acts as a link between components, the Entity is a simple object model, and the Router manages screen creation and navigation.

### The VIPER template
The first step will be to access the folder *~/Library/Developer/Xcode/Templates/File Templates* and create a new folder that we will name *VIPER.xctemplate*.

For VIPER, I work with a template in which I have created five files:

__FILEBASENAME__Protocols.swift

__FILEBASENAME__ViewController.swift

__FILEBASENAME__Presenter.swift

__FILEBASENAME__Interactor.swift

__FILEBASENAME__Router.swift

Therefore, when we create a screen called, for example, *Login*, the following files should appear: *LoginProtocols*, *LoginViewController*, *LoginPresenter*, *LoginInteractor* and *LoginRouter*.

We must also have the *TemplateInfo.plist* file, which we can copy from the ones that Xcode has and modify them according to our needs.
In this file, together with the template description, we add the different files that will be created when using it (in the image you can see the case of the file that contains the protocols).

{{< image src="images/posts/xcode_templates_1.png" alt="Xcode TemplateInfo.plist">}}


Now that we have this file, all we have to do is complete each of the created files with code. In the case of VIPER, I have customized this code based on the needs of the project.

### __FILEBASENAME__Protocols.swift
```swift
//
//  ___FILENAME___
//  ___PROJECTNAME___
//

import UIKit

// MARK: Router Input
protocol PresenterToRouter___VARIABLE_moduleName___Protocol {
    static func createScreen() -> UIViewController
}

// MARK: View Input
protocol ViewToPresenter___VARIABLE_moduleName___Protocol {
    var view: PresenterToView___VARIABLE_moduleName___Protocol? { get set }
    var interactor: PresenterToInteractor___VARIABLE_moduleName___Protocol? { get set }
    var router: PresenterToRouter___VARIABLE_moduleName___Protocol? { get set }
}

// MARK: View Output
protocol PresenterToView___VARIABLE_moduleName___Protocol {}

// MARK: Interactor Input
protocol PresenterToInteractor___VARIABLE_moduleName___Protocol {
    var presenter: InteractorToPresenter___VARIABLE_moduleName___Protocol? { get set }
}

// MARK: Interactor Output
protocol InteractorToPresenter___VARIABLE_moduleName___Protocol {}
```

### __FILEBASENAME__ViewController.swift
```swift
//
//  ___FILENAME___
//  ___PROJECTNAME___
//

import UIKit

class HomeViewController: UIViewController {
    
    var presenter: ViewToPresenter___VARIABLE_moduleName___Protocol!
    
    override func loadView() {
        super.loadView()
        setupHomeView()
        presenter.viewDidLoad()
    }
    
    private func setupHomeView() {

    }
}
```

### __FILEBASENAME__Presenter.swift
```swift
//
//  ___FILENAME___
//  ___PROJECTNAME___
//

import Foundation

class ___VARIABLE_moduleName___Presenter: ViewToPresenter___VARIABLE_moduleName___Protocol {
    
    var view: PresenterToView___VARIABLE_moduleName___Protocol?
    var interactor: PresenterToInteractor___VARIABLE_moduleName___Protocol?
    var router: PresenterToRouter___VARIABLE_moduleName___Protocol?
}
```

### __FILEBASENAME__Interactor.swift
```swift
//
//  ___FILENAME___
//  ___PROJECTNAME___
//

import Foundation

class ___VARIABLE_moduleName___Interactor: PresenterToInteractor___VARIABLE_moduleName___Protocol {
    
    var presenter: InteractorToPresenter___VARIABLE_moduleName___Protocol?

}
```

### __FILEBASENAME__Router.swift
```swift
//
//  ___FILENAME___
//  ___PROJECTNAME___
//

import UIKit

class ___VARIABLE_moduleName___Router: PresenterToRouter___VARIABLE_moduleName___Protocol {
    
    static func createScreen() -> UIViewController {
        
        let presenter: ViewToPresenter___VARIABLE_moduleName___Protocol & InteractorToPresenter___VARIABLE_moduleName___Protocol = ___VARIABLE_moduleName___Presenter()
        
        let viewController = ___VARIABLE_moduleName___ViewController()
        viewController.presenter = presenter
        viewController.presenter.router = ___VARIABLE_moduleName___Router()
        viewController.presenter?.view = viewController
        viewController.presenter?.interactor = ___VARIABLE_moduleName___Interactor()
        viewController.presenter?.interactor?.presenter = presenter
        return viewController
    }
}
```

Now, if we go to Xcode and create a new file, in the templates section we will find the one we have created.

{{< image src="images/posts/xcode_templates_2.png" alt="Xcode VIPER Template">}}

And when selecting it, we can indicate the name of the module (in this case Login) so that it creates the corresponding files for us.

{{< image src="images/posts/xcode_templates_3.png" alt="Xcode VIPER Module classes creation">}}


## VIP
The VIP architecture is a software design pattern in which data flows unidirectionally between the View, Interactor, and Presenter in a cycle.
The View is in charge of managing the scene and communicating events to the Interactor, the Interactor is in charge of business logic and data retrieval, and the Presenter converts information received from the Interactor into data that the View can display. The Router is an optional component that is responsible for navigating between view controllers and passing information, and depending on the complexity of the screen, the Model and Worker components may also be present.

#### The VIP template
In this case we will do the same as when we created the template for VIPER, but now we will have some different files:

__FILEBASENAME__Configurator.swift

__FILEBASENAME__ViewController.swift

__FILEBASENAME__View.swift

__FILEBASENAME__Presenter.swift

__FILEBASENAME__Interactor.swift

__FILEBASENAME__Router.swift

__FILEBASENAME__Model.swift

Therefore, when we create a screen called, for example, *Login*, the following files should appear: *LoginConfigurator.swift*, *LoginViewController.swift*, *LoginView.swift*, *LoginPresenter.swift*, *LoginInteractor.swift*, *LoginRouter.swift*, *LoginModel.swift*.

Along with the new *TemplateInfo.plist*, we can put all these files in a folder we call *VIP.xctemplate*.

For VIP based projects, I have customized a template with the following code (but remember, it depends on the project we are working on and can be customized as needed).

### __FILEBASENAME__Configurator.swift
```swift
//
//  ___FILENAME___
//  ___PROJECTNAME___
//

import Foundation

final class ___VARIABLE_sceneName___Configurator {
    
    static func configure( _ viewController: ___VARIABLE_sceneName___ViewController) -> ___VARIABLE_sceneName___ViewController {
        
        let interactor = ___VARIABLE_sceneName___Interactor()
        let presenter = ___VARIABLE_sceneName___Presenter()
        let router = ___VARIABLE_sceneName___Router()
        router.viewController = viewController
        presenter.viewController = viewController
        interactor.presenter = presenter
        viewController.interactor = interactor
        viewController.router = router
        return viewController
    }
}
```

### __FILEBASENAME__ViewController.swift
```swift
//
//  ___FILENAME___
//  ___PROJECTNAME___
//

import UIKit

protocol ___VARIABLE_sceneName___ViewControllerInput: class {}

protocol ___VARIABLE_sceneName___ViewControllerOutput: class {}

final class ___VARIABLE_sceneName___ViewController: UIViewController {
    var interactor: ___VARIABLE_sceneName___InteractorInput?
    var router: ___VARIABLE_sceneName___RouterDelegate?
    
    private let sceneView: ___VARIABLE_sceneName___View
    
    init(sceneView: ___VARIABLE_sceneName___View) {
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

extension ___VARIABLE_sceneName___ViewController: ___VARIABLE_sceneName___ViewControllerInput {}

extension ___VARIABLE_sceneName___ViewController: ___VARIABLE_sceneName___ViewDelegate {}
```
{{<ads1>}}


### __FILEBASENAME__View.swift
```swift
//
//  ___FILENAME___
//  ___PROJECTNAME___
//

import UIKit

protocol ___VARIABLE_sceneName___ViewDelegate: class {}

final class ___VARIABLE_sceneName___View: UIView {
    
    weak var delegate: ___VARIABLE_sceneName___ViewDelegate?

    override init(frame: CGRect) {
        super.init(frame: frame)
        configureUI()
    }
    
    required init?(coder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
    
    func configureUI() {}
}
```

### __FILEBASENAME__Presenter.swift
```swift
//
//  ___FILENAME___
//  ___PROJECTNAME___
//

import UIKit

typealias ___VARIABLE_sceneName___PresenterInput = ___VARIABLE_sceneName___InteractorOutput
typealias ___VARIABLE_sceneName___PresenterOutput = ___VARIABLE_sceneName___ViewControllerInput

final class ___VARIABLE_sceneName___Presenter {
    weak var viewController: ___VARIABLE_sceneName___PresenterOutput?
}

extension ___VARIABLE_sceneName___Presenter: ___VARIABLE_sceneName___PresenterInput {}

```

### __FILEBASENAME__Interactor.swift
```swift
//
//  ___FILENAME___
//  ___PROJECTNAME___
//

import Foundation

protocol ___VARIABLE_sceneName___InteractorOutput: class { }

typealias ___VARIABLE_sceneName___InteractorInput = ___VARIABLE_sceneName___ViewControllerOutput

final class ___VARIABLE_sceneName___Interactor {
    var presenter: ___VARIABLE_sceneName___PresenterInput?
}

extension ___VARIABLE_sceneName___Interactor: ___VARIABLE_sceneName___InteractorInput {}
```

### __FILEBASENAME__Router.swift
```swift
//
//  ___FILENAME___
//  ___PROJECTNAME___
//

import UIKit

protocol ___VARIABLE_sceneName___RouterDelegate {}

final class ___VARIABLE_sceneName___Router {
    weak var viewController: UIViewController?
}

extension ___VARIABLE_sceneName___Router: ___VARIABLE_sceneName___RouterDelegate {}

```

### __FILEBASENAME__Model.swift
```swift
//
//  ___FILENAME___
//  ___PROJECTNAME___
//

import Foundation

enum ___VARIABLE_sceneName___Model {
    
    enum Model {
        struct Request {}
        struct Response {}
        struct ViewModel {}
    }
}
```

Now, if we go to Xcode and create a new file, in the templates section we will find the one we have created.

{{< image src="images/posts/xcode_templates_4.png" alt="Xcode VIP Template">}}

And when selecting it, we can indicate the name of the module (in this case Login) so that it creates the corresponding files for us.

{{< image src="images/posts/xcode_templates_5.png" alt="Xcode VIP Module classes creation">}}
{{<ads2>}}

# Conclusion
Finally, using **Xcode templates** can greatly simplify the development process and eliminate repetitive tasks. Developers can save time, increase efficiency, and ensure consistency across projects by creating custom templates. Using Xcode templates to increase productivity and maintain high-quality code is a must-have tool for developers.