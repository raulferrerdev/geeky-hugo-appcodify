---
title: "Advanced Swift Testing: Mocking and Stubbing"
description: "An explanation of advanced testing methods in Swift, such as mocking and stubbing, and how to utilize them to create tests that are more useful. Additionally covered are examples and recommended practices for testing complex or difficult-to-predict behavior with mock objects and stubs, testing asynchronous code, and decoupling code from its dependencies."
date: 2018-03-05
categories: ["Swift", "Testing"]
tags: ["Development"]
draft: false
---

## Advanced Swift Testing
As you gain experience testing with Swift, you could encounter problems that are difficult to resolve using [simple **test cases**](https://raulferrer.dev/blog/starting_unit_testing_in_swift/). You might need to employ sophisticated **Swift testing** methods like **mocking** and **stubbing** in certain circumstances. In this piece, we'll look at **mocking** and **stubbing** and how you can use them to create tests that are more useful.
{{<ads1>}}

## What is mocking?
Using the **mocking** technique, you can make *fake objects that can be substituted for actual ones in your tests*. You may isolate the piece of code you're testing and make sure it operates as you anticipate using these fictitious objects, often known as mocks.

A *mocking* framework like *Mockito* or *OCMock* is generally used to construct mocks. These frameworks enable you to define the behavior of any object or class as mocks in your tests.

### Why mocking?
You might want to utilize **mocking** in your **tests** for a number of reasons:

* To separate and test a single piece of code. When your code is dependent on other objects or services, this is especially helpful.
* To test software that communicates with outside sources like databases or APIs. You can replicate these interactions using **mocks** instead of sending genuine network requests.
* To examine code that exhibits complex or unpredictable behavior. Instead of relying on the actual behavior of the object, **mocks** let you specify the precise behavior you want to test.

## What is stubbing?
**Stubbing** is a method for *specifying how an object or function behaves in your tests*. **Stubs** give you the same ability as mocks to isolate a piece of code and specify its behavior for the needs of your tests.

In order to **stub** an object or method, you often *create a subclass of it* and modify its behavior. The *@testable* property in Swift enables you to make your code testable and gives you access to and control over private methods and properties.

### Why stubbing?
You might wish to **stub** in your tests for a number of reasons:
* To examine code that exhibits complex or unpredictable behavior. Instead than relying on the actual behavior of the object, stubs let you specify the precise behavior you want to test.
* To evaluate edge cases and error handling. Stubs let you push your code into particular states so you can test how it reacts.
* To test software that makes use of databases or other external APIs. You can emulate these interactions using stubs rather than sending genuine network requests.

## Testing a view controller with mocks and stubs
Let's look at an example of how to test a view controller using **mocks** and **stubs**. Consider a view controller that shows a movie list retrieved from a remote API. To make sure that the view controller is accurately displaying the list of movies, we want to create a test.

We'll start by building a *dummy* API client. The APIClient class will be **mocked** using the *Mockito* framework, and its behavior will be specified in our tests. Here is how our mockup might appear:

```swift
class MockAPIClient: APIClient {
    var movies: [Moview] = []
    
    override func fetchMovies(completion: @escaping ([Movie]?, Error?) -> Void) {
        completion(movies, nil)
    }
}
```
The **stub** for our view controller will be made next. To set the users property to a specified list of movies, we will construct a subclass of UIViewController and override the loadMovies method. Here is an example of our **stub**:

```swift
@testable
class StubViewController: ViewController {
    override func loadMovies() {
        let movies = [Movie(id: 1, title: "Riders of the Lost Ark"), Movie(id: 2, name: "Star Wars")]
        self.movies = movies
    }
}
```

Our **test case** can now be written. We will construct a **mock** APIClient instance for our **test case** and set it as the apiClient attribute of our *dummy* UIViewController. Then, we'll use the loadMovies method to check that the view controller's movies property is set to what is required. Here is an example of our test case:

```swift
func testLoadMovies() {
    let apiClient = MockAPIClient()
    apiClient.movies = [Movie(id: 1, title: "Riders of the Lost Ark"), Movie(id: 2, name: "Star Wars")]
    
    let viewController = StubViewController()
    viewController.apiClient = apiClient
    
    viewController.loadMovies()
    
    XCTAssertEqual(viewController.movies, apiClient.movies)
}
```
The **stub** UIViewController in this test defines the behavior of the view controller while the **mock** APIClient simulates the behavior of the real API client. This enables us to independently test and validate the loadMovies method's functionality.
{{<ads2>}}

## Conclusion
In this post, we learnt about advanced **Swift testing** strategies like **mocking** and **stubbing** and how to utilize them to create Swift tests that are more useful. Testing complex or hard-to-predict behavior is made simpler by the use of **mocks** and **stubs**, which enable us to isolate particular code units and specify their behavior for the purposes of our tests. We watched a demonstration of how to test a view controller that pulls data from a remote API using **mocks** and **stubs**.

