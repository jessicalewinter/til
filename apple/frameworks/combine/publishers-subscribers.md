# Publishers and Subscribers
To create a basic publisher in Combine, you can transform any value type using the `.publisher`, that will convert this type to a publisher that emits elements when start its subscription

```swift
let fibonacciArray = [1, 2, 3, 5, 8].publisher
```

The type of this publisher it is `Publishers.Sequence<[Int], Never>`

Publishers is a enum that contains Combine's built-in publishers.
Every publisher in Combine has an Output and a Failure.

On our example, the subscribers of a sequence publisher will receive Int objects when subscring to it. This sequence publisher receives as a failure the Never type, this means it will never emit error events, only successfull events.



## References

