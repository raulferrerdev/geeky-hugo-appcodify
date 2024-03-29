---
title: "Testing UINavigationController and UITabBarController in Swift"
description: "In this work, we discussed different techniques for testing an UINavigationController or UITabBarController using XCTest in Swift. We covered four main techniques: setting up the controller in the test method, creating a subclass of the controller, using a mock object, and using a partial mock."
date: 2018-07-02
categories: ["Swift", "Testing"]
tags: ["Development"]
draft: false
---

We discussed various advanced testing strategies for Swift in a [recent article](https://raulferrer.dev/blog/advanced_testing_in_swift_mocking_stubbing/), including mocks and stubs. Although we have seen some examples of these strategies in use, there are times when we may encounter cases that are more complex.
What methods might we employ, for instance, if we wanted to test a **UINavigationController** or a **UITabBarController** component?
{{<ads1>}}

# Testing UINavigationController
You have a several alternatives when using Swift's XCTest to test a **UINavigationController**'s behavior.

## Configuring the UINavigationController
The **UINavigationController** can be created and configured as part of your test procedure. If you wish to put up particular criteria or data for your test, this can be helpful.

```swift
func testNavigationController() {
    let navController = UINavigationController()
    let rootViewController = UIViewController()
    navController.viewControllers = [rootViewController]

    XCTAssertEqual(navController.viewControllers.count, 1)
    XCTAssertEqual(navController.topViewController, rootViewController)
}
```
## Making a subclass
You can make a subclass of **UINavigationController** and use it in your tests to replace certain methods or properties. This can be helpful if you wish to simulate specific **UINavigationController** behavior while testing.

```swift
class TestNavigationController: UINavigationController {
    var didCallPushViewController = false
    
    override func pushViewController(_ viewController: UIViewController, animated: Bool) {
        didCallPushViewController = true
    }
}

func testNavigationController() {
    let navController = TestNavigationController()
    let rootViewController = UIViewController()
    navController.viewControllers = [rootViewController]
    
    navController.pushViewController(UIViewController(), animated: false)
    
    XCTAssertTrue(navController.didCallPushViewController)
}
```
## Using a mocking object
In your tests, you may use a mock object to imitate the **UINavigationController**'s behavior. If you want to ensure that your code is interacting with the **UINavigationController** as you anticipate, this can be helpful.

```swift
class MockNavigationController: UINavigationController {
    var didCallPushViewController = false
    
    override func pushViewController(_ viewController: UIViewController, animated: Bool) {
        didCallPushViewController = true
    }
}

func testNavigationController() {
    let mockNavController = MockNavigationController()
    let rootViewController = UIViewController()
    mockNavController.viewControllers = [rootViewController]
    
    let navController = UINavigationController()
    navController.pushViewController(rootViewController, animated: false)
    
    XCTAssertTrue(mockNavController.didCallPushViewController)
}
```
## Using a partial mock
In your tests, you can make use of a partial fake for the **UINavigationController**. This enables you to mock only specific **UINavigationController** methods or attributes while still using a genuine instance of the object in your tests.
```swift
class PartialMockNavigationController: UINavigationController {
    var didCallPushViewController = false
    
    override func pushViewController(_ viewController: UIViewController, animated: Bool) {
        didCallPushViewController = true
    }
}

func testNavigationController() {
    let navController = PartialMockNavigationController()
    let rootViewController = UIViewController()
    navController.viewControllers = [rootViewController]
    
    navController.pushViewController(UIViewController(), animated: false)
    
    XCTAssertTrue(navController.didCallPushViewController)
}
```

It's important to pick the best technique for your particular testing requirements. You should take into account aspects like the complexity of your tests, the level of granularity you require in your fake objects, and the amount of setup necessary when choosing one of these strategies because each one has advantages and disadvantages.

## Some tips on testing UINavigationController
Hera are son points to consider when testing UINavigationViewController:

* It's generally a good idea to start a new instance of the **UINavigationController** for each test when configuring your test environment. This makes it easier to make sure your tests are separate from one another and don't conflict.

* To break the connection between your test code and the **UINavigationController**, you might want to think about using dependency injection. This can make it simpler to test various scenarios because you can insert dummy objects or **UINavigationController** setups into your code.

* Make sure to check that your test code is calling or accessing the appropriate methods or properties if you are using a mock object or partial mock of the **UINavigationController**. To accomplish this, use **XCTAssert** statements.

* When testing a **UINavigationController**, it's often a good idea to test a range of different scenarios. You might want to test, for instance, what happens when you pop the top view controller off the stack or put a new view controller onto it.

# Testing UITabBarController

As with **UINavigatinoContorller**, here are several methods you may employ to test a **UITabBarController** in Swift using **XCTest**.

## Configuring the UITabBarController
The **UITabBarController** can be created and configured as part of your test method. If you wish to put up particular criteria or data for your test, this can be helpful.

```swift
func testTabBarController() {
    let tabBarController = UITabBarController()
    let viewController1 = UIViewController()
    let viewController2 = UIViewController()
    tabBarController.viewControllers = [viewController1, viewController2]
    
    XCTAssertEqual(tabBarController.viewControllers.count, 2)
}
```
## Making a subclass
You can make a subclass of **UITabBarController** and use it in your tests to replace certain methods or properties. This can be helpful if you wish to simulate specific **UITabBarController** behavior while testing.

```swift
class TestTabBarController: UITabBarController {
    var didSelectViewController = false
    
    override func tabBar(_ tabBar: UITabBar, didSelect item: UITabBarItem) {
        didSelectViewController = true
    }
}

func testTabBarController() {
    let tabBarController = TestTabBarController()
    let viewController1 = UIViewController()
    let viewController2 = UIViewController()
    tabBarController.viewControllers = [viewController1, viewController2]
    
    tabBarController.selectedIndex = 1
    
    XCTAssertTrue(tabBarController.didSelectViewController)
}
```
## Using a mocking object
In your tests, you may use a mock object to imitate the **UITabBarController**'s behavior. If you want to ensure that your code is interacting with the **UITabBarController** as you anticipate, this can be helpful.

```swift
class MockTabBarController: UITabBarController {
    var didSelectViewController = false
    
    override func tabBar(_ tabBar: UITabBar, didSelect item: UITabBarItem) {
        didSelectViewController = true
    }
}

func testTabBarController() {
    let mockTabBarController = MockTabBarController()
    let viewController1 = UIViewController()
    let viewController2 = UIViewController()
    mockTabBarController.viewControllers = [viewController1, viewController2]
    
    let tabBarController = UITabBarController()
    tabBarController.selectedIndex = 1
    
    XCTAssertTrue(mockTabBarController.didSelectViewController)
}
```

## Using a partial mock
In your tests, you can make use of a partial mock for the **UITabBarController**. As a result, you can mock only a subset of the **UITabBarController**'s methods or properties while still using a real instance of the object in your tests.

```swift
class PartialMockTabBarController: UITabBarController {
    var didSelectViewController = false
    
    override func tabBar(_ tabBar: UITabBar, didSelect item: UITabBarItem) {
        didSelectViewController = true
    }
}

func testTabBarController() {
    let tabBarController = PartialMockTabBarController()
    let viewController1 = UIViewController()
    let viewController2 = UIViewController()
    tabBarController.viewControllers = [viewController1, viewController2]
    
    tabBarController.selectedIndex = 1
    
    XCTAssertTrue(tabBarController.didSelectViewController)
}
```
## Some tips on testing UITabBarController
Here are some extra things to think about while testing a **UITabBarController** in Swift with **XCTest**:

* It's generally a good idea to start a new instance of the **UITabBarController** for each test when configuring your test environment. This makes it easier to make sure your tests are separate from one another and don't conflict.

* Use dependency injection to break the connection between your test code and the **UITabBarController**. As you can insert dummy objects or other **UITabBarController** configurations into your code, this can make it simpler to test various scenarios.

* Verify that the right methods or properties are being called or accessed by your test code if you're using a mock object or partial mock of the **UITabBarController**. To accomplish this, use **XCTAssert** statements.

* When testing a **UITabBarController**, it's often a good idea to test a range of various scenarios. You might wish to test, for instance, what happens when you open a different tab or when you alter the things in the tab bar.

* Make careful to test the **UITabBarController**'s behavior as well as that of the view controllers it controls. To ensure that the correct view controllers are being displayed and acting as intended, use **XCTest**.
{{<ads2>}}

# Conclusion
In conclusion, using **XCTest** in Swift to test a **UINavigationController** or **UITabBarController** can be a useful approach to make sure that your code is functioning properly and acting as expected. The controller can be set up in the test function, a subclass of the controller can be created, a mock object can be used, and a partial mock can be used, among other approaches, to test these controllers.

Whichever technique you select, it's crucial to carefully analyze the particular needs of your testing and select the approach that best satisfies those objectives. You may feel confident in the accuracy of your code by testing a variety of scenarios and confirming the behavior of both the controllers and the view controllers that they oversee.