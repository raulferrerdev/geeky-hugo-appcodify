---
title: "Mastering Concurrency in Swift (I): Processes an threads"
description: "Start to learn about concurrency in Swift with processes and threads."
date: 2023-04-01
categories: ["Swift", "Concurrency"]
tags: ["Development"]
type: "regular" # available types: [featured/regular]
draft: true
---
## Concurrency in Swift
**Concurrency** is an essential element of modern software development, allowing developers to create applications that are efficient, responsive, and scalable. In the case of Swift, this simultaneity allows us to write efficient code that can take advantage of multi-core processors and provide a smooth user experience. With the introduction of new **concurrency** functions in Swift 5.5 and SwiftUI, such as **async/await** and structured **concurrency**, it is easier to write concurrent code in a safe and efficient manner.

In this series of articles, we will look at the fundamentals of Swift **concurrency**, such as processes, subprocesses, and blocking, as well as advanced topics like **Grand Central Dispatch (GCD)**, **Operation Queues**, and Swift's new **concurrency** functions.
{{<ads1>}}


## Introduction to Prcesses and Threads
**Processes** and **threads** are basic concepts in actual operating systems such as macOS and iOS, and understanding how they work is essential for developing efficient and responsive software. In a nutshell:

* A **process** is a container that encapsulates an application and its associated resources.
* A **thread** is a lightweight execution unit that can operate concurrently with other threads within a process.

Developers can build apps that can perform multiple tasks at the same time by leveraging processes and threads, providing a more seamless user experience and taking advantage of multi-core processors.

## Processes
### What are processes

A **process** is a running program instance on an operating system:

* Each **process** has its own memory area, complete with code, data, and a stack.
* A **process** can spawn numerous execution threads, each with its own stack and program counter.
* Interprocess communication (IPC) mechanisms such as pipes, sockets, and shared memory allow processes to interact with one another.

For example, each running program in macOS is treated as a separate process. When you launch the Safari browser, for example, a new process is launched for the program.


### Processes use cases

#### Isolation
**Processes** offer separation between various applications or various components of a single application. Because each process has its own memory space, it is impossible for one process to access the memory of another.

```swift
import Foundation

// This function forks a new process and prints a message from the parent process
func forkProcess() {
    let pid = fork()
    if pid < 0 {
        print("Error forking process")
    } else if pid == 0 {
        // This is the child process
        print("Child process started")
    } else {
        // This is the parent process
        print("Parent process started")
    }
}

forkProcess()
```

In this example, a new process is started using the *fork()* method. The function returns 0 in the child process and the process ID (PID) of the child process in the parent process. Creating a new process by forking allows the child process to have its own memory space and function separately from the parent process.

#### Resource allocation
Each process has its own resources, like as network sockets and file handles, which can be distributed independently. This makes resource allocation by the operating system more effective.
```swift
import Foundation

// This function creates a new file and writes some data to it
func createFile() {
    let fileURL = URL(fileURLWithPath: "/tmp/example.txt")
    FileManager.default.createFile(atPath: fileURL.path, contents: "Hello, world".data(using: .utf8), attributes: nil)
}

createFile()
```

Here, a new file is created on disk using the *createFile()* method and the *FileManager* class. The program is allocating a new resource that is distinct from other resources in the system by creating a new file.
{{<ads2>}}


#### Fault tolerance

If a process crashes or hangs, it doesn't affect other processes running on the system.
```swift
import Foundation

// This function simulates a crash by dereferencing a null pointer
func simulateCrash() {
    let pointer: UnsafeMutablePointer<Int> = nil
    pointer.pointee = 42
}

simulateCrash()
```

In this example, the *simulateCrash()* method dereferences a null pointer, resulting in an application crash and a segmentation fault. The crash will not have an impact on other processes operating on the system because the application is running in its own process.



## What are threads

