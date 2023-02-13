# Intro to RxSwift

## What is RxSwift
RxSwift is a library for composing asynchronous and event-based code by using observable sequences and functional style operators, allowing for parameterized execution via schedulers.


### Asynchronous programming glossary
#### Which problems RxSwift solves?
### State, and specifically, shared mutable state
multiple places changing the same data
##### Imperative programming
order imports
##### Side effects
the app behaves one way before executing someMethod, and differently after that.
##### Declarative code
Declarative code lets you define pieces of behavior, and RxSwift will run these behaviors any time there’s a relevant event and provide them an immutable, isolated data input to work with.
##### Reactive systems
- **Responsive**: Always keep the UI up to date, representing the latest app state
- **Resilient**: Each behavior is defined in isolation and provides for flexible error recovery
- **Elastic**: The code handles varied workload, often implementing features such as lazy pull-driven data collections, event throttling, and resource sharing.
- **Message driven**: Message-based communication for improved reusability and isolation.


## Core RX

### Observables
Observable<T> has the ability to asynchronously produce a sequence of events that can carry an immutable snapshot of data T. 
In few words, it allow classes to subscribe for values emitted by another class over time.

The Observable<T> class allows one or more observers to react to any events in real time and update the app UI.

The ObservableType protocol is extremely simple. An Observable can emit (and observers can receive) only three types of events

- Next event: An event which brings the latest(or "next") data value. This is the way observers receive these values
- Completed event: This events finished the event sequence with success. it means the Observable completes its life-cycle with sucess and won't emit any ohther events. It stops right there.
- error event: The observable ends with an error and won't emit any other events further.

#### Finite observable sequences
Some observable sequences emit zero, one, or more values, and at a later point, either terminate successfully or terminate with an error.

#### Infinite observable sequences
Has sequences which are simply infinite. Often, UI events are such infinite observable sequences.

### Operators
The ObservableType include plenty of methods that abstract discrete pieces of asynchronous work, which can be composed together to implement more complex logic

```
APIClient.request(url)
   .filter { value in
      return value.isProfessor
   }
   .map { data in
      return "Mama \(data)"
   }
   .subscribe(onNext: { data in
     self.presenter.showUsers(data)
   })

```

The operators are highly composable — they always take in data as input an output their result, so you can easily chain them in many different ways achieving so much more than what a single operator can do on its own!

### Schedulers
Schedulers are the Rx equivalent of DispatchQueues, but with more powers
RxSwift comes with a number of predefined schedulers, which cover most of the use cases and this probably means that you will never have to go about creating your own scheduler

SerialDispatchQueueScheduler -> GCD on specific serial queue
ConcurrentDispatchQueueScheduler -> GCD concurrent queue
OperationQueueScheduler ->NSOperationQueue

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

