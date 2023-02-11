# Intro to RxSwift

## What is RxSwift


### Subscription
- Triggers an observable to begin emmiting event until `.error` or `.completed` is emmited
- Observing an observable is known as "subscribing".
- Subscript

### Subject
Contains any number of dependent observers
Implements both the Observable and Observer Interfaces

```swift
let source = PublisherSubject<String>()
source.map { x in ...}.filter {x in ...}.subscribe(onNext: { x in ...})
source.onNext("first") //emits data
source.onNext("second")
```

### Subscription


## References


## References

