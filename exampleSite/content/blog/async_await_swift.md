---
title: "Async/Await use in Swift and SwiftUI"
description: "Async/await is a language feature that allows you to write asynchronous code in a synchronous-looking style. "
date: 2023-01-02
categories: ["Swift", "Concurrency"]
tags: ["Development", "Code"]
image: "https://drive.google.com/uc?id=14gxuTvOLzWNzsy7gvFS4yt6dIddPxPiF"
draft: true
---

A language feature called **async/await**, enables you to construct asynchronous code that seems to be synchronous. It enables concurrent task execution without the use of convoluted callback procedures, making concurrent task execution code easier to create and read.

Utilizing **async/await** has a number of advantages, one of which is that it makes your software more effective and responsive. When you utilize **async/await**, you may create programs that conduct time-consuming processes in the background while letting the main thread carry on. This guarantees that your software will remain responsive while it waits for a task to finish, which can be crucial in user-facing apps.

**Async/await** can enhance the developer experience in addition to making your code more effective and responsive. Developers used to writing synchronous code will find it easier to grasp and more familiar to create asynchronous code in this manner. This may lessen the learning curve for dealing with asynchronous code and facilitate developers' assimilation.
##### Async/Await syntaxis

The **async** keyword designates an *asynchronous* function that will employ the **await** operator. When an asynchronous task is supplied, the await operator is used to delay the function's execution until it has finished.

In order to use **async/await** we must define an **async** function and utilize the **await** operator inside of it. For example:

```swift
func request() async {
    let result = await makeRequest()
    // do something with result
}
```
The function *request()* should be marked as **async** as we are calling an **await**-using function inside of it.

The function must be marked with the async keyword and called with the await operator in order to use the await operator. So, in order to use the *request()* function, we need to use the **await** opeator:
```swift
let resquestResult = await request()
```

##### Adding try-catch
When we use **async/await**, we will most likely find ourselves needing to handle errors, so we will have to use **try-catch**. Adding this case is very simple, for example:

```swift
import Foundation

func request() async throws -> Data {
    // make a call that is asynchronous and that can return data or throw an error
}

func fetchData() async throws -> String {
    do {
        let data = try await request()
        let json = try JSONSerialization.jsonObject(with: data) as? [String: Any]
        let object = json["object"] as? String ?? "No data found"
        return object
    } catch {
        throw error
    }
}
```
The *request()* function is, on the one hand, *asynchronous*, so we add the **async** operator, but on the other hand, it can also *throw errors*, so we add **throws** to it. To call the *request()* function we must use **try** and **await** as is shown.

If any errors are thrown in this instance while it is running, they are caught in the catch block and rethrown. Then, using the await operator, the fetchData function can be executed, and any errors it may throw can be handled in a try-catch block.

```swift
do {
    let object = try await fetchData()
    print(object)
} catch {
    print("An error occurred: \(error.localizedDescription)")
}
```

##### Removing closures
One benefit of using **async/await** that makes it possible to write asynchronous code without the usage of challenging *callback* methods. This may result in easier to write and maintain, simpler, more succinct code.
For example, let see how the above example of request looks with *callback* methods (closures):

```swift
func request(completion: (Result<Data, Error>) -> Void) {
    // make the request and call completion with the result
}

func fetchData() {
    request { result in
        switch result {
        case .success(let data):
            // handle the data
        case .failure(let error):
            // handle the error
        }
    }
}

```

##### Legacy code transformation

If we have to deal with old code we can mainly follow two ways to transform it to **async/await**: creating it again or using a wrapper.

Supose:
func callAPI() async -> Result<Data, Error> {
    let url = URL(string: "https://www.example.com")!
    let semaphore = DispatchSemaphore(value: 0)
    var result: Result<Data, Error>!
    let task = URLSession.shared.dataTask(with: url) { (data, response, error) in
        if let error = error {
            result = .failure(error)
        } else if let data = data, let response = response as? HTTPURLResponse {
            result = .success(data)
        }
        semaphore.signal()
    }
    task.resume()
    semaphore.wait()
    return result
}

###### New methods
This case is based on developing new methods from scratch with async/await functionality. By creating new methods, we make sure that code based on previous methods doesn't break and everything works correctly.


Wrapping methods