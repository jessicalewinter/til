# Opaque types
Opaque types allow you to describe the expected return type without defining a concrete type.
At first, it looks like we’re returning a protocol type. Though, the some keyword is crucial here as it allows the compiler to access the actual type information and performoptimizations.

You could say we’re giving the compiler more information than just stating we’re expecting a View protocol to be returned by appending the some keyword.

You can use the reserved keyword `some` to create opaque types

By using opaque types we were able to remove the need to expose code publically, allowing us to refactor code internally without publishing breaking changes.
 Gaining this flexibility is crucial during normal development with internal APIs and when providing frameworks.

## Replace generics reference
```swift
    func printElement<T: CustomStringConvertible>(_ element: T) {
        print(element)
    }

    func printElement(element: some CustomStringConvertible) {
        print(element)
    }
```

## References
- [What are opaque types](https://www.avanderlee.com/swift/some-opaque-types/)
- [Opaque Types](https://docs.swift.org/swift-book/LanguageGuide/OpaqueTypes.html)
