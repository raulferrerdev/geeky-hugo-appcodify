---
title: "Model-View-Presenter (MVP) architecture on iOS"
description: "Model-View-Presenter is an architectural pattern that derives from another well-known pattern, Model-View-Controller (widely used in the development of iOS applications), in which a new component, the Presenter, acts as an intermediary between the View and the Model."
date: 2017-10-19
categories: ["Swift", "Architecture"]
tags: ["Development"]
draft: false
---

# Introduction
**MVP (Model-View-Presenter)** is an architectural pattern derived from MVC, which is frequently used in the creation of mobile applications. This pattern allows you to separate presentation logic from business logic. This separation helps make the code more maintainable, scalable, and testable.

# Components
The MVP has three components:
* **Model.** Represents data and business logic.

* **View.** It's the user interface. Also contains de Controller (UIVIewController).

* **Presenter.** It acts as a bridge between the *Model* and the **View**. On the one hand, it receives the events of the **View** and passes them to the **Model** and, on the other hand, it updates the **View** when the data changes in the **Model**.

The **Presenter** receives user input from the **View**, processes it, and updates the **Model**. In turn, the Model notifies the **View** of any changes to the data.

{{< image src="images/posts/mvp_schema_1.png" alt="MVP Schema">}}


# Example code

Now we are going to see a simple example of applying the **MVP** pattern.

To do this, let's create a small app with a single screen that will have a TextField, a Button and a Label. When we add a username in the TextField and click on the button, the label will show *"My name is:* " and the name that we have entered.

## Model
The **Model** is very simple, it is a *struct* with a single parameter: the *name* of the user.

```swift
struct User {
    var name: String
}
```

## Presenter
The **Presenter** has a reference to the **View** but, to avoid a strong reference cycle, is a *weak* reference. A strong reference cycle occurs when two objects have strong references to each other, resulting in an unbreakable retain cycle. Because the objects cannot be deallocated by the system, this can result in a memory leak.

```swift
class UserPresenter {
    
    weak var userView: UserViewDelegate?
    var user = User(name: "")

    init(userView: UserViewDelegate) {
        self.userView = userView
    }

    func updateUserName(name: String) {
        user.name = name
        userView?.showNameForUser(user: user)
    }
}
```
By holding a weak reference to the **View**, the **Presenter** can only communicate with it as long as it exists. That is, when the **View** is destroyed, the weak reference becomes null and the **Presenter** can no longer access said **View**. This helps ensure that memory is managed correctly in the application.

## View and UIViewController

The UIViewController is in charge of instantiating both the **View** and the **Presenter**. Once both are instantiated, the **View** is passed to the **Presenter** and the **Presenter** is also assigned to the view to establish communication.

Finally, the **View**'s *setupView* method is called to create its components and is assigned to the UIViewController's view.

```swift
class UserViewController: UIViewController {
    
    var userView = UserView()
    
    init() {
        super.init(nibName: nil, bundle: nil)
    }
    
    required init?(coder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
    
    override func loadView() {
        super.loadView()
        setupView()
    }
    
    func setupView() {
        let presenter = UserPresenter(userView: userView)
        userView.presenter = presenter
        userView.setupView()
        self.view = userView
    }
}
```
In the **View**, we can see the reference to the **Presenter** (which we assign from the UIViewContrioller) and how this reference is used to call the **Presenter**'s *updateUserName(name:)* method to update the model.
Once updated, the **Prensenter** (as we have seen) calls the **View**'s *showNameForUser(user: User)* method thanks to the delegate we have created.

```swift
protocol UserViewDelegate: class {
    func showNameForUser(user: User)
}

class UserView: UIView, UserViewDelegate {
    
    let nameTextField = UITextField()
    let updateNameButton = UIButton()
    let userNameLabel = UILabel()
    
    var presenter: UserPresenter!
    
    override init(frame: CGRect) {
        super.init(frame: frame)
        setupView()
    }
    
    required init?(coder: NSCoder) {
        fatalError("init(coder:) has not been implemented")
    }
    
    func setupView() {
        backgroundColor = .white
        
        nameTextField.frame = CGRect(x: 20, y: 100, width: 200, height: 44)
        nameTextField.borderStyle = .roundedRect
        addSubview(nameTextField)
        
        updateNameButton.frame = CGRect(x: 20, y: 150, width: 200, height: 44)
        updateNameButton.setTitle("Update Name", for: .normal)
        updateNameButton.backgroundColor = .blue
        updateNameButton.addTarget(self, action: #selector(updateNameButtonTapped), for: .touchUpInside)
        addSubview(updateNameButton)
        
        userNameLabel.frame = CGRect(x: 20, y: 200, width: 200, height: 44)
        userNameLabel.backgroundColor = .lightGray
        addSubview(userNameLabel)
    }
    
    @objc func updateNameButtonTapped() {
        presenter.updateUserName(name: nameTextField.text ?? "")
    }
    
    func showNameForUser(user: User) {
        userNameLabel.text = "My name is: \(user.name)"
    }
}
```
# Advantages and Disadvantages of MVP

## Advantages
* Compared to the MVC pattern, it offers a greater separation of duties.
* Although it is a bit more complex than MVC, it is derived from it, so we can quickly get used to using it.
* Business logic tests could be improved.

## Disadvantages
* Because it's more complex than MVC, it's typically not recommended for use in small, simple applications.
* The Presenter in MVP has the same potential to become an important component as the Controller in MVC.
* Even though we have further modularized the design, there are still some issues, such as continuous controller control over screen switching.

# Conclusion

**MVP (Model-View-Presenter)** is an architectural pattern that separates the application's presentation layer from its business logic. This allows for a clear separation of responsibilities and reduces component coupling. The View in **MVP** serves as a passive interface that displays data and forwards user actions to the **Presenter**, while the **Presenter** serves as a mediator between the **View** and the **Model**, updating the **View** based on **Model** changes and forwarding user actions to the **Model**. The **Model** represents the application's data and business logic.

