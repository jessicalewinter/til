# Publishers and Subscribers
To create a basic publisher in Combine, you can transform any value type using the `.publisher`, that will convert this type to a publisher that emits elements when start its subscription

```swift
let fibonacciArray = [1, 2, 3, 5, 8].publisher
```

The type of this publisher it is `Publishers.Sequence<[Int], Never>`

Publishers is a enum that contains Combine's built-in publishers.
Every publisher in Combine has an Output and a Failure.

On our example, the subscribers of a sequence publisher will receive Int objects when subscring to it. This sequence publisher receives as a failure the Never type, this means it will never emit error events, only successfull events.



## Subscribing

### receiveValue shorthand
`sink` comes with a shorthand for publishers that have Never as their Failure type. It allows us to omit the receiveCompletion closure:
[1, 2, 3].publisher.sink(receiveValue: { value in
  print("received a value: \(value)")
})
This shorthand version of sink only works for publishers that never fail. If your publisher has the possibility of failing, you need to add the `receiveCompletion` closure.

### assign built-in subscriber
The assign subscriber is also defined on Publisher and it allows us to directly assign publisher values to a property on an object:

```swift
var contact = Contact()
["lastName"].publisher.assign(to: \.lastName, on: contact)
```

The preceding code creates a publisher that emits strings. We use assign to directly assign every published string from this publisher to the user object’s email property. This can be very convenient but it comes with some rules. The assign method requires that the key path that we want to assign values to is a ReferenceWriteableKeyPath. This pretty much means that the key path must belong to a class. And for this to work, we need to expose the target property to the Objective-C runtime using an @objc annotation. 
 Contact class:

```swift
class Contact {
  @objc var lastName = "default"
}
```

## References

