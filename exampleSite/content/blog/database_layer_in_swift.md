---
title: "Data access layer in Swift"
description: "Learn how to separate the data access layer of an application from the rest of the components, so that you can change its type without the need for major code changes."
date: 2018-12-20
categories: ["Swift", "Architecture"]
tags: ["Development", "Code"]
type: "regular" # available types: [featured/regular]
draft: false
---
## Data access
Nowadays,for **data access** most mobile applications have an internal database (Core Data, Realm …) to store information, which can then be used, for example, if the application does not have an internet connection.

{{<ads1>}}

If for **data access** we want to implement a database in our application following SOLID principles, we must take into account:

* The database layer should not be exposed to controllers (ViewControllers).
* CRUD operations in the database can only be called from a service class.
* The database layer should allow changing the type of database in the future (for example, from Core Data to Realm), affecting as little as possible the application code.
* The objects or entities of the database must not be exposed outside the database layer.

## StoreManager protocol

First, we must establish a protocol that declares the methods for interacting with the database. To decouple the type of object that these methods receive with the type of database, we will establish a protocol, Storable, that all objects must adopt:

```swift
protocol Storable { }

extension Object: Storable { } // For Realm database
extension NSManagedObject: Storable { } // For Core Data database
```

Now we can develop the StorageManager protocol:

```swift
protocol StorageManager {
  /// Save Object into Realm database
  /// - Parameter object: Realm object (as Storable)
  func save(object: Storable) throws

  /// Save array of objects into Realm database
  /// - Parameter objects: Array of Realm objects (as Storable)
  func saveAll(objects: [Storable]) throws

  /// Update object on Realm database
  /// - Parameter object: Realm object (as Storable)
  func update(object: Storable) throws

  /// Delete Object on Realm database
  /// - Parameter object: Realm object (as Storable)
  func delete(object: Storable) throws

  /// Delete all objects of a given Realm type
  /// - Parameter model: Realm model (as Storable)
  func deleteAll(_ model: Storable.Type) throws

  /// Fetch objects
  /// - Parameters:
  ///   - model: Realm object model (as Storable)
  ///   - predicate: Predicate
  ///   - sorted: Sorted object (as Storable)
  func fetch(_ model: Storable.Type,
             predicate: NSPredicate?,
             sorted: Sorted?) -> [Storable]

  /// Delete all objects in database
  func reset() throws
}
```

Where *Sorted* is a struct that allows us to sort the results obtained through a specific parameter (*key*):

```swift
struct Sorted {
  var key: String
  var ascending: Bool = true
}
```

{{<ads2>}}

## Conclusion

On **data access**, we can decouple de database layer of our apps, following SOLID principles. This will allow us, for example, change database type in our app with few changes.