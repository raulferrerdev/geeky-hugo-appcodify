---
title: "Check Internet status with NWPathMonitor"
description: "Learn how to use NWPathMonitor to control the connectivity of your application (to know Internet status)."
date: 2018-11-11
categories: ["Connectivity", "Swift"]
tags: ["Development", "Code"]
type: "regular" # available types: [featured/regular]
draft: false
---

## Checking Internet status in an app
With iOS 12, Apple has introduced Network, a framework that includes the **NWPathMonitor** class. **NWPathMonitor** gives us the means to monitor changes of state in the **Internet status** (so it is no longer necessary to use the Reachability class, in applications that support iOS 12 onwards). Therefore, we can set aside the Reachability library, and detect the state of the network with **NWPathMonitor**.
{{<ads1>}}


## NWPathMonitor
To use this new way to check **Internet status**, we first need to create an instance of **NWPathMonitor**:

```swift
let monitor = NWPathMonitor()
```


We can also instantiate the **NWPathMonitor** class indicating a particular type of network that we want to check. For example, to check WiFi connections:

```swift
let monitor = NWPathMonitor(requiredInterfaceType: .wifi)
```

NWPathMonitor can check different types of interfaces:

* **cellular.** For 3G / 4G connections.
* **loopback.** For localhost.
* **other.** For virtual networks or unknown network types.
* **wifi.** For WiFi connections.
* **wiredEthernet.** If the device is connected to the internet by cable.

Detection of changes in the **Internet status** are made through the *pathUpdateHandler* property:

```swift
monitor.pathUpdateHandler = { path in
    if path.status == .satisfied {
        print("!Hay conexión con internet!")
    }
}
```

path is of *NWPath* type and, according to Apple documentation, status can be:

* **unsatisfied.** The connection (path) cannot be used.
* **satisfied.** The connection (path) has been established and allows data to be sent.
* **requiresConnection.** The connection (path) is not currently available, but if a new connection is established it can be activated.

For more information on the possibilities of *NWPath* we can access the Apple documentation.
Finally, in order to start receiving information about changes in the state of the internet connection, we need to call the *start()* method. The *start()* method needs a queue to do this job:

```swift
let queue = dispatchQueue.global(qos: .background)
monitor.start(queue: queue)
```


Once we no longer need to know the changes in the state of the internet connection, we will call the cancel () method.
### Code example

For example, suppose we want to know at all times the status of the internet connection throughout the application. For this we can use a class of type Singleton:

```swift
import Foundation
import Network

class NetMonitor {
    static public let shared = NetMonitor()
    private var monitor: NWPathMonitor
    private var queue = DispatchQueue.global()
    var netOn: Bool = true
    var connType: ConnectionType = .wifi

    private init() {
        self.monitor = NWPathMonitor()
        self.queue = DispatchQueue.global(qos: .background)
        self.monitor.start(queue: queue)
    }

    func startMonitoring() {
        self.monitor.pathUpdateHandler = { path in
            self.netOn = path.status == .satisfied
            self.connType = self.checkConnectionTypeForPath(path)
        }
    }

    func stopMonitoring() {
        self.monitor.cancel()
    }

    func checkConnectionTypeForPath(_ path: NWPath) -> ConnectionType {
        if path.usesInterfaceType(.wifi) {
            return .wifi
        } else if path.usesInterfaceType(.wiredEthernet) {
            return .ethernet
        } else if path.usesInterfaceType(.cellular) {
            return .cellular
        }

        return .unknown
    }
}
```


Where *ConnectionType* is an enum that contains the specific cases of connectivity for our app. For example:

```swift
public enum ConnectionType {
    case wifi
    case ethernet
    case cellular
    case unknown
}
```
{{<ads2>}}


To use this Singleton type class:

```swift
let netConnection = NetMonitor.shared
netConnection.start() // Internet connection monitoring starts
netConnection.cancel() // Internet connection monitoring stops
let status = netConnection.connType // Returns the connection type
```
