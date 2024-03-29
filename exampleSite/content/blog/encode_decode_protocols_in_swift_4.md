---
title: "Encode/Decode protocols in Swift 4 (Introduction)"
description: "Learn how to use the Encode and Decode protocols to pass information in JSON format to a struct or class and vice versa."
date: 2017-11-25
categories: ["Swift"]
tags: ["Development", "Code"]
type: "regular" # available types: [featured/regular]
draft: false
---
Currently, many applications receive information from the servers with which they connect in JSON format.
With Swift 4, this process has been simplified thanks to the Encodable and Decodable protocols. The struct or class that adopt these protocols may be encoded to a JSON format or decoded from that format, with a few lines of code.

{{<ads1>}}

* *Encodable protocol.* Convert instances of a specific type to format, for example, JSON.
* *Decodable protocol.* Converts information in format, for example, JSON to instances of a certain type.
* *Codable protocol.* It is a typealias definition for a protocol that adopts Encodable and Decodable protocols.

```swift
typealias Codable = Encodable & Decodable
```

A struct or class may adopt the Codable type if its properties are also of the Codable type:

String, Int, Double, Data, URL
Array, Dictionary, Optional (if contain Codable types)

# Decodable

Let’s see now how to work with the Decodable protocol. We will assume that our application calls a web service that returns the information of a specific user in JSON format:

```swift
let userJson = """
{
  "id": "39631383-4e2a-48ef-bb40-f9d896392eab",
  "firstName": "Jane",
  "lastName": "Doe",
  "mail": "jane.doe@example.com",
  "active": "true"
}
"""
```

To pass this information to an instance of our struct User:

```swift
struct User: Codable { // User conforms to Decodable protocol
  let id: String
  let firstName: String
  let lastName: String
  let mail: String
  let active: String
}

let jsonData = userJson.data(using: .utf8)! // Json is converted to Data type
do {
    let user = trey JSONDecoder().decode(User.self, from: jsonData) // Data is decoded
    print(user)
} catch {
    print(error.localizedDescription)
}
```
{{<ads2>}}

# Encodable

If we want to obtain a JSON from an instance of our struct User:

```swift
var user = User()
user.id = "39631383-4e2a-48ef-bb40-f9d896392eab"
user.firstName = "Jane"
user.lastName = "Doe"
user.mail = "jane.doe@example.com"
user.active = "true"

do {
    let jsonData = try JSONEncoder().encode(user)
    print(String(decoding: jsonData, as: UTF8.self))
} catch {
    print(error)
}
```
