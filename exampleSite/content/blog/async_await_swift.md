---
title: "Async Await use in Swift and SwiftUI"
description: "Async/await is a language feature that allows you to write asynchronous code in a synchronous-looking style. "
date: 2023-01-06
categories: ["Swift", "Concurrency"]
tags: ["Development", "Code"]
image: "https://drive.google.com/uc?id=14gxuTvOLzWNzsy7gvFS4yt6dIddPxPiF"
draft: false
---

## Async await in Swift

A language feature called **async await**, enables you to construct asynchronous code that seems to be synchronous. It enables concurrent task execution without the use of convoluted callback procedures, making concurrent task execution code easier to create and read.
{{<ads1>}}

Utilizing **async await** has a number of advantages, one of which is that it makes your software more effective and responsive. When you utilize **async await**, you may create programs that conduct time-consuming processes in the background while letting the main thread carry on. This guarantees that your software will remain responsive while it waits for a task to finish, which can be crucial in user-facing apps.

**Async await** can enhance the developer experience in addition to making your code more effective and responsive. Developers used to writing synchronous code will find it easier to grasp and more familiar to create asynchronous code in this manner. This may lessen the learning curve for dealing with asynchronous code and facilitate developers' assimilation.
## Async Await syntaxis

The **async** keyword designates an *asynchronous* function that will employ the **await** operator. When an asynchronous task is supplied, the await operator is used to delay the function's execution until it has finished.

In order to use **async await** we must define an **async** function and utilize the **await** operator inside of it. For example:

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

### Adding try-catch to async await
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

### Removing closures
One benefit of using **async await** that makes it possible to write asynchronous code without the usage of challenging *callback* methods. This may result in easier to write and maintain, simpler, more succinct code.
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

### Legacy code transformation

If we have to deal with old code we can mainly follow two ways to transform it to **async await**: creating it again or using *withCheckedContinuation/withCheckedThrowingContinuation*.

For example, let's say we have the following code to download an image:

```swift
func downloadImage(from url: URL, to fileURL: URL, completion: @escaping (Result<Void, Error>) -> Void) {
    let request = URLRequest(url: url)
    let dataTask = urlSession.dataTask(with: request) { data, response, error in
        if let error = error {
            completion(.failure(error))
        } else if let data = data {
            do {
                try data.write(to: fileURL)
                completion(.success(()))
            } catch {
                completion(.failure(error))
            }
        } else {
            completion(.failure(URLError(.badServerResponse)))
        }
    }
    dataTask.resume()
}
```
We can write a new method with pure **async/await**:

```swift
func downloadImage(from url: URL, to fileURL: URL) async throws {
    let request = URLRequest(url: url)
    let (data, _) = try await urlSession.dataTask(with: request)
    try data.write(to: fileURL)
}
```
But, we can refactor the legacy to **async/await** using *withCheckedContinuation/withCheckedThrowingContinuation*:

```swift
func downloadImage(from url: URL, to fileURL: URL) async throws {
    let request = URLRequest(url: url)
    try await withCheckedThrowingContinuation { continuation in
        let dataTask = urlSession.dataTask(with: request) { data, response, error in
            if let error = error {
                continuation.resume(throwing: error)
            } else if let data = data {
                do {
                    try data.write(to: fileURL)
                    continuation.resume(returning: ())
                } catch {
                    continuation.resume(throwing: error)
                }
            } else {
                continuation.resume(throwing: URLError(.badServerResponse))
            }
        }
        dataTask.resume()
    }
}
```

#### withCheckedThrowingContinuation

**withCheckedThrowingContinuation** is a function that creates a continuation that can be resumed with a throwing expression. It is used in conjunction with the **await** keyword to enable asynchronous error handling.

Only **async** functions are permitted to utilize the **await** keyword, which pauses the execution of the function until an awaited job is finished. The task's outcome is returned if it is successfully completed. The caller of the **async** function receives the error if the task throws one. 

You can make a continuation that can be resumed with a throwing expression by using the **withCheckedThrowingContinuation** function, which enables you to throw errors from within the continuation block. This is advantageous when working with asynchronous tasks that could fail since it enables you to transmit any faults to the async function's caller.

Here is an illustration of how to use **withCheckedThrowingContinuation**:

```swift
try await withCheckedThrowingContinuation { continuation in
    // Perform some asynchronous task
    if someErrorOccurs {
        continuation.resume(throwing: someError)
    } else {
        continuation.resume(returning: someResult)
    }
}
```
The asynchronous task is carried out in this case within the continuation block. In the event of an error, the continuation is continued with a throwing expression, which propagates the error to the async function's caller. If the task succeeds, the continuation is continued with a returning expression that gives the caller the task's outcome.

#### withCheckedContinuation

A continuation created by the function **withCheckedContinuation** can be picked up by a returning expression. Despite not allowing you to throw errors from within the continuation block, it is comparable to **withCheckedThrowingContinuation**.

```swift
let result = try await withCheckedContinuation { continuation in
    // Perform some asynchronous task
    continuation.resume(returning: someResult)
}
```
The asynchronous task is carried out in this case within the continuation block. A returning expression that returns the task's outcome to the caller resumes the continuation when the task is finished.
{{<ads2>}}

## Conclusion

It is simpler to write and comprehend code that does tasks concurrently because to the strong language feature known as **async await**, which enables you to express asynchronous code in a synchronous-looking manner. It may result in applications that are more responsive and effective, as well as a better developer experience. Swift's implementation of **async await** makes use of special function types, the await operator, and can be combined with try-catch blocks to handle errors. You can build asynchronous code that is simpler, clearer to understand, and easier to maintain than code that uses closures by using **async await**.