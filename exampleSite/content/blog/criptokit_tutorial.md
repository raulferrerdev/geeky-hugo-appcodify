---
title: "Cybersecurity info. How to use CryptoKit on iOS13 apps"
description: "Cryptokit is an Apple library that allows us to work with encryption (cybersecurity info) in our applications, generate public and private keys... Look in this post at some of its most interesting possibilities."
date: 2020-02-17
categories: ["Framework", "Swift"]
tags: ["CryptoKit", "Code"]
type: "regular" # available types: [featured/regular]
draft: false
---

## Using the CriptoKit framework

If you're concerned about **cybersecurity info**, in this article we are going to see an introduction tutorial to [CryptoKit](https://developer.apple.com/videos/play/wwdc2019/709/), presented by Apple on WWDC19, and how it can be used in applications developed for iOS13. **CryptoKit** allows, for example, the exchange of public and private keys, so that you could get to make purchases from an iPhone with the cryptocurrencies (Bitcoin, for example) stored in it.

{{<ads1>}}


**CryptoKit** allows developers:

* Create and compare hashes.
* Message encryption and authentication using symmetric cryptography.
* Use of public keys to create and evaluate digital signatures.

The use of **CryptoKit** is simple and does not require low level API knowledge or manually memory management.
## Main operations that can be performed with CryptoKit
### Hashing

Hash functions generate, in general, a unique key from some input data. While the input data is the same, the key obtained will be the same, which allows you to use them, for example, to know if a file has been modified or not (if it has been modified, its hash will be different).
**CryptoKit** allows, through the use of the HashFunction protocol, to obtain a hash according to three different implementations:

* SHA256
* SHA384
* SHA512

```swift
let data = string.data(using: .utf8)!
let hash256 = SHA256.hash(data: data)
let hash384 = SHA384.hash(data: data)
let hash512 = SHA512.hash(data: data)
```

In this way we obtain hash values in a simpler way than in the past:

```swift
func sha256(stringValue: String) -> String {
  let data = stringValue.data(using: String.Encoding.utf8)!
  var digest = [UInt8](repeating: 0, count: Int(CC_SHA1_DIGEST_LENGTH))
  CC_SHA256((data as NSData).bytes, CC_LONG(data.count), &digest)
  let hexBytes = digest.map { String(format: "%02hhx", $0) }
  return hexBytes.joined(separator: "")
}
```

Apple also provides access to some algorithms that it considers unsafe, such as MD5 and SHA1, for backward compatibility reasons. For this we must use [enum Insecure](https://developer.apple.com/documentation/cryptokit/insecure):

```swift
let md5Hash = Insecure.MD5.hash(data: data)
let sha1Hash = Insecure.SHA1.hash(data: data)
```

### Create and validate digital signatures

Digital signatures are used to verify the authenticity of a message or data. This procedure is done many times daily, either in email, in financial transactions or when accessing https pages.
**Cryptokit** supports four different ways to create and verify digital signatures:

* Curve25519
* P521
* P384
* P256

To use this system, we first need to generate a public and a private key:

```swift
let privateKey = P384.Signing.PrivateKey()
let publicKey = privateKey.publicKey
let publicKeyData = publicKey.rawRepresentation
```

To sign content we use the private key (which also includes the public key) that we have obtained:

```swift
if let signature = try? privateKey.signature(for: data) {
  // We can use signature
}
```


With the public key we can check if the digital signature is valid or not:

```swift
if publicKey.isValidSignature(signature, for: data) {
  print("Valid signature")
}
```

### Data encryption

Nowadays data encryption is of great importance, since it allows your confidentiality. **CryptoKit** supports two types of encryption algorithms:

* AES-GCM
* ChaChaPoly (this is preferred in mobile environments because it is faster).

We can do the encryption and decryption of data in a simple way:

```swift
// Generates a symetric ramdom key
let randomKey = SymmetricKey(size: .bits256)

// Encrytion
if let cryptedBox = try? ChaChaPoly.seal(data, using: randomKey), let sealedBox = try? ChaChaPoly.SealedBox(combined: cryptedBox.combined) {
   // Do whatever with cryptedBox
}

// Decryption
if let combinedData = sealedBox.combined, let sealedBoxToOpen = try? ChaChaPoly.SealedBox(combined: combinedData), let decryptedData = try! ChaChaPoly.open(sealedBox, using: randomKey), let decryptedString = String(data: decryptedData, encoding: .utf8) {
   print(decryptedString)
}
```

In this case, in the data encryption we obtain an object of type ChaChaPoly.SealedBox (which is a data container that can only be accessed with the encryption key). This object includes the combined property, which in turn contains three properties:

* *nonce*: it is an arbitrary number used in data encryption.
* *ciphertext*: are the encrypted data, with the same size as the input data.
* *tag*: it is an authentication label and prevents data from being altered without us noticing.

### Key sharing

A key agreement protocol is a process during which two parties securely choose a shared encryption key that can be used to sign and encrypt the data they want to share with each other. This also allows unauthorized parties to access the data.

First, we generate a value for the salt, that is, a value that comprises random bits that are used as one of the inputs in a key derivative function. For example, we can get a value as follows:

```swift
if let salt = "Our Value For Salt".data(using: .utf8) {
    // Derive keys using the salt value
}
```


Now, both parties can publish and share with each other their public key (user A shares his public key are user B, and vice versa):

```swift
let userAPrivateKey = P521.KeyAgreement.PrivateKey()
let userAPublicKey = userAPrivateKey.publicKey

let userBPrivateKey = P521.KeyAgreement.PrivateKey()
let userBPublicKey = userBPrivateKey.publicKey
```

Next, user A shares a secret with his private key and the public key of user B. User B must do the same and verify that the keys obtained are the same:

```swift
if let userASharedSecret = try? userAPrivateKey.sharedSecretFromKeyAgreement(with: userBPublicKey) {
    let userASymmetricKey = userASharedSecret.hkdfDerivedSymmetricKey(using: SHA256.self, salt: salt, sharedInfo: Data(), outputByteCount: 32)
}

if let userBSharedSecret = try? userBPrivateKey.sharedSecretFromKeyAgreement(with: userAPublicKey) {
    let userBSymmetricKey = userBSharedSecret.hkdfDerivedSymmetricKey(using: SHA256.self, salt: salt, sharedInfo: Data(), outputByteCount: 32)
}

if userASymmetricKey == userBSymmetricKey {
  print("Keys are equal. Let's share data.")
}
```

Now we can use the generated symmetric key to encrypt information as we have seen previously (either through the AES-GCM algorithm or the ChaChaPoly).

{{<ads2>}}

## Conclusion

Respect to **cybersecurity info**, unlike the previous system used in Apple to encrypt information (CommonCrypto), **CryptoKit** is high level, so it is simple to use. It includes the most common encryption algorithms, as well as the most used operations: hashing, encryption and key sharing.
