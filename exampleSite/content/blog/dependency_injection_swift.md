---
title: "Dependency injection in Swift"
description: "If you use dependency injection, your application code will become more decoupled, testable, and scalable."
date: 2020-11-01
categories: ["Architecture", "Swift"]
tags: ["Development", "Code"]
type: "regular" # available types: [featured/regular]
draft: false
---
## Introduction to Dependency injection

If you are like me, when developing an application you are always looking (although sometimes it is difficult) to use a series of ‘rules’ that help you to make the code cleaner, be as uncoupled as possible, be scalable…

In a previous article I already talked about [SOLID](https://codderlly.com/blog/solid_principles_swift/) principles and how to apply them with Swift. In this article I am going to discuss a pattern that will allow us to reduce the coupling of our code and that will make it more easily testable: **Dependency Injection**.

{{<ads1>}}
## But, what is dependency injection?

If we go directly to the definition that [Wikipedia](https://en.wikipedia.org/wiki/Dependency_injection) gives us, we find the following:

> In software engineering, **dependency injection** is a technique in which an object receives other objects that it depends on. These other objects are called dependencies. In the typical “using” relationship the receiving object is called a client and the passed (that is, “injected”) object is called a service. The code that passes the service to the client can be many kinds of things and is called the injector. Instead of the client specifying which service it will use, the injector tells the client what service to use. The “injection” refers to the passing of a dependency (a service) into the object (a client) that would use it.

The advantages of **dependency injection** in Swift are:

* Reduce coupling.
* The code is reusable.
* The code is testable.
* Facilitates code maintenance.

## Dependency injection example

For example, suppose that we are preparing a screen in which the user can log into the application, but for this we need to validate it on an internet server.

If we are using the MVVM pattern and in our ViewModel class we use a class that manages the internet connection, we can initially do the following:

```swift
protocol NetworkManagerProtocol {
    func login(username: String, password: String, completion: @escaping (Data, Error) -> Void)
}

class loginViewModel {

    var networkManager: NetworkManagerProtocol = NetworkManager()

    private func loginWith(name: Stirng, password: String) {
        networkManager.login(username: name, password: password, ©ompletion: { (data, error) in
            guard error != nil else {
                // Deal with error
            return
            }

            // Authorize login
        })
    }
}
```


This code works fine, but has some problems:

* First of all, it is the LoginViewModel class itself that creates a NetworkManager instance. This makes the dependency and coupling between LoginViewModel and NetworkManager high.
* When creating the NetowrkManager instance inside the LoginViewModel, it becomes virtually impossible to test the LoginViewModel independently (eg by passing a mock up for connectivity).

To solve this situation, what we must do is modify the code of the LoginViewModel class so that we can pass it the NetworkManager instance:

```swift
protocol NetworkManagerProtocol {
    func login(username: String, password: String, completion: @escaping (Data, Error) -> Void)
}

class loginViewModel {

    var networkManager: NetworkManager

    init(networkManager: NetworkManagerProtocol) {
        self.networkManager = networkManager
    }

    private func loginWith(name: Stirng, password: String) {
        self.networkManager.login(username: name, password: password, ©ompletion: { (data, error) in
            guard error != nil else {
                // Deal with error
            return
            }

            // Authorize login
        })
    }
}
```

## Dependency injection testing

Now, for example, if we want to test the LoginViewModel class we can pass a ‘mocked’ NetworkManager instance. That is, we can create our own class that has the behavior that we want. The new class simply has to comply with the NetworkManagerProtocol protocol:

```swift
class NetworManagerMock: ProductManagerProtocol {
    func login(username: String, password: String, completion: @escaping (Data, Error) -> Void) {
        completion(data, nil)
    }
}
```


Or, if we want to test the errors:

```swift
class NetworManagerMock: ProductManagerProtocol {
    func login(username: String, password: String, completion: @escaping (Data, Error) -> Void) {
        completion(nil, error)
    }
}
```

```swift
class LoginViewModelTests: XCTestCase {
    func testLogiViewModelWithData() {
        let networkManagerMock = NetworManagerMock()
        let loginViewModel = LoginViewModel(networkManager: networkManagerMock)
        let expectation = self.expectation(description: "Networking")
        let loginData: Data
        
        networkManager.loginWith(username: "Marc", password: "1234", ©ompletion: { (data, error) in
            loginData = data
            expectation.fulfill()
        })

        waitForExpectations(timeout: 5, handler: nil)
        XCTAssertNotNil(data)
    }
}
```

## Types of dependency injection

Within the injection of dependencies we can distinguish three types:

* Initializer injection
* Property injection
* Method injection

### Initializer injection

It is the type of initialization that we have seen in the example. In this case we pass the dependency during object initialization. Also, it has the advantage that the dependency is immutable.

```swift
class loginViewModel {

    var networkManager: NetworkManager

    init(networkManager: NetworkManagerProtocol) {
        self.networkManager = networkManager
    }
}
```

### Property injection

Injection of a dependency can also be done by assigning it as a parameter. In this case, this assignment makes the property not immutable, since then we could modify it with a new assignment:

```swift
class loginViewModel {
    var networkManager: NetworkManager
}

// Implementation
let loginViewModel = LoginViewModel()
loginViewModel.neworkManager = NetworkManager()
```
{{<ads2>}}


### Method injection

Another way to inject a dependency is to do it whenever we want. This can be done by creating a method that allows dependency injecting.

```swift
class loginViewModel {

    func authorizeWith(networkManager: NetowrkManagerProtocol) -> Data? {
        // Deal with data
    }
}
```

## Conclusion

Knowing about dependency injection in Swift will allow us to create more decoupled code, with a lower tendency to have errors and whose maintenance will be easier.
