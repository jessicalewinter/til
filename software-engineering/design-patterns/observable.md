# Observable Pattern
A behavioral desing pattern

Let's define a subscription mechanism to notify multiple objects about any events that happen to the object they're observing

## Terminology
### Observable

Observable is whom deliver values to the "consumer", a.k.a, the "observer"

- It can be transformed and subscribed

### Observer
Observer is a consumer of values delivered by an Observable
Feeds an observable source
who emits data

### Observer vs Observable
An "observer" is a class or struct that subscribes to an "observable" and receives events.
Could be also called by the subscriber method on RxSwift, just like this:

```swift
observable.map { x in ...}
          .filter { x in ...}
          .subscribe(onNext: { x in ...}) // onNext closue is the observer in this case
```

### Subject
Contains any number of dependent observers
Implements both the Observable and Observer Interfaces



## References

