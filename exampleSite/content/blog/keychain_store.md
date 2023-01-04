---
title: "Securely Store and Retrieve Sensitive Data with the Keychain in Swift"
description: "Learn how to use the keychain in your iOS, macOS, tvOS, or watchOS app to safely store and retrieve passwords, encryption keys, and other sensitive data. Learn how to share data between apps using the keychain and how to use iCloud to back up your data. Swift examples that walk you through each step will show you how to use the keychain in your own apps."
date: 2023-01-04
categories: ["Swift", "Security"]
tags: ["Development", "Code"]
image: "https://drive.google.com/uc?id=1rgluJG_cPLPZ2xJ8utoidu7NDx1PJLqA"
draft: false
---

A safe approach to persist small amounts of data in your iOS, macOS, tvOS, or watchOS app is to store it in the **keychain**. A database called **keychain** is used to store private data such as encryption keys and passwords.

In this article, we'll look at utilizing Swift to save data in the **keychain**. We'll start by talking about the keychain and its significance before delving into the procedures needed to store information in the keychain.

##### What exactly is a keychain, and why is it significant?
Passwords, encryption keys, and other private data are kept in the keychain, a secure database. The keychain is a secure location to keep sensitive information because it is encrypted and secured by the user's device passcode.

The keychain's ability to be shared throughout different apps on the same device is one of its main advantages. This implies that you don't need to keep a password in numerous locations because you can access it from other apps by storing it in the keychain.

The fact that the keychain is backed up by iCloud means that it is included in the backup of the user's device when that device is backed up to iCloud. This implies that if the user loses their device or installs a new one, the keychain can be restored.

##### Swift-based keychain data storage
Swift users must import the Security framework and utilize the SecItemAdd function in order to store data in the keychain.

An illustration of how to store a password in the keychain is as follows:

```swift
import Security

class KeychainStoreManager {
        func store(password: String, account: String, service: String) {
        let passwordData = password.data(using: .utf8)!
        let accountData = account.data(using: .utf8)!
        let serviceData = service.data(using: .utf8)!
        
        let query: [String: Any] = [
            kSecClass as String: kSecClassGenericPassword,
            kSecAttrAccount as String: accountData,
            kSecAttrService as String: serviceData,
            kSecValueData as String: passwordData
        ]
        
        let status = SecItemAdd(query as CFDictionary, nil)
        
        if status == errSecSuccess {
            print("Password saved successfully to keychain.")
        } else {
            print("Error saving password to keychain: \(status)")
        }
    }
}
```

* **kSecClass** is the key used to define the kind of item being kept in the keychain. In the instances, we indicate that we are saving a generic password in the keychain by using the value *kSecClassGenericPassword*.

* **kSecAttrAccount** is the key used to define the account linked to the object that is being kept in the keychain. The value *accountData*  is the Data object representation of the account string used in the examples. That is, identifies the item stored.

* **kSecAttrService** is used to specify the service linked to the keychain item (i.e, the bunflew identifier).

* **kSecValueData** is used to indicate the information that will be kept in the keychain, as the value *passwordData*, which is the Data object representation of the password string used in the examples.

In the preceding example, we convert the password and account strings to Data objects first. The dictionary with the key-value pairs required to store the password in the keychain is then created. The keys are *Security* framework constants, and the values are the Data objects we created earlier.

The dictionary is then passed to the *SecItemAdd* function, which adds the password to the keychain. The function returns a status code that indicates whether or not the operation was successful.

##### Getting information from the keychain
The *SecItemCopyMatching* function can be used to retrieve data from the keychain. Here's an example of how to retrieve a keychain password:

```swift
func getPassword(account: String, service: String) {
    let accountData = account.data(using: .utf8)!
    let serviceData = service.data(using: .utf8)!
        
    let query: [String: Any] = [
        kSecClass as String: kSecClassGenericPassword,
        kSecAttrAccount as String: accountData,
        kSecAttrService as String: serviceData,
        kSecReturnData as String: true,
        kSecMatchLimit as String: kSecMatchLimitOne
    ]
        
    var result: AnyObject?
    let status = SecItemCopyMatching(query as CFDictionary, &result)
        
    if status == errSecSuccess {
        if let passwordData = result as? Data,
            let password = String(data: passwordData, encoding: .utf8) {
            print("Password retrieved successfully: \(password)")
        } else {
            print("Error retrieving password from keychain.")
        }
    } else {
            print("Error retrieving password from keychain: \(status)")
    }
}
```

* **kSecReturnData** is used to indicate that the information related to the keychain item being retrieved should be returned. In this example, we specify that the data should be returned by using the value true.

* **kSecMatchLimit** specifies the maximum number of things that should be returned.
In this example, we specify that only one item should be returned by using the value *kSecMatchLimitOne*. 

In the example, we create a dictionary containing the key-value pairs needed to retrieve the password from the keychain. The keys are *Security* framework constants, and the values are the *account* and *service* Data objects and the *true*'* value for the **kSecReturnData*'* key.

The dictionary is then passed to the *SecItemCopyMatching*'* function, which retrieves the password from the keychain. The function returns a status code that indicates whether or not the operation was successful. If the operation was successful, the returned password Data object is converted to a String and printed to the console.

##### Uptating information in the keychain

The *SecItemUpdate*'* function can be used to update data in the keychain. Here's an example of how to update a keychain password:

```swift
func update(password: String, account: String, service: String) {
    let passworData = password.data(using: .utf8)!
    let accountData = account.data(using: .utf8)!
    let serviceData = service.data(using: .utf8)!
        
    let query: [String: Any] = [
        kSecClass as String: kSecClassGenericPassword,
        kSecAttrAccount as String: accountData,
        kSecAttrService as String: serviceData
    ]
        
    let update: [String: Any] = [
        kSecValueData as String: passworData
    ]
        
    let status = SecItemUpdate(query as CFDictionary, update as CFDictionary)
    if status == errSecSuccess {
        print("Password updated successfully in keychain.")
    } else {
        print("Error updating password in keychain: \(status)")
    }
}
```

In the example, we create a dictionary containing the key-value pairs needed to update the password in the keychain. The keys are *Security* framework constants, and the values are the *account* and *service* Data objects and the new password Data object.

The dictionary is then passed to the **SecItemUpdate* function, which updates the password in the keychain. The function returns a status code that indicates whether or not the operation was successful.

##### Deleting information from the keychain

The *SecItemDelete* function can be used to delete data from the keychain. Here's an example of how to delete a keychain password:

```swift
func deletePassword(account: String, service: String) {
    let accountData = account.data(using: .utf8)!
    let serviceData = service.data(using: .utf8)!
        
    let query: [String: Any] = [
        kSecClass as String: kSecClassGenericPassword,
        kSecAttrAccount as String: accountData,
        kSecAttrService as String: serviceData
    ]
        
    let status = SecItemDelete(query as CFDictionary)
    if status == errSecSuccess {
        print("Password deleted successfully from keychain.")
    } else {
        print("Error deleting password from keychain: (status)")
    }
}
```
In this example, we create a dictionary containing the key-value pairs needed to remove the password from the keychain. The keys are *Security* framework constants, and the values are the *account* and *service* Data objects.

The dictionary is then passed to the *SecItemDelete* function, which deletes the password from the keychain. The function returns a status code that indicates whether or not the operation was successful.

##### Conclusion
In this post, we learned how to use Swift to store, retrieve, update, and delete data from the keychain. We saw how the keychain can be shared among multiple apps on the same device and is a secure and convenient way to persist small pieces of data in your app.
