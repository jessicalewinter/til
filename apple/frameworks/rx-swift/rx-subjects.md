# Subjects

## What is a subject?
Subjects act as both an observable and an observer. 
The subject received .next events, and every time it receives an event, it turns around and emits it to its subscribers.

```swift
    let subject = PublishSubject<String>()
    subject.onNext("hello baby")
    let subscriber = subject.subscribe(onNext: { event in
      print(event)
    })
    
    subject.on(.next("1"))
    subject.onNext("2")

    // Console
    // "hello baby"
    // "1"
    // "2"
    // The onNext event "hello baby" is not emiited because is only emits current subscriber, the emmited events only shows up after uyou subscribe your subject
```

 *on(.next(_:))* is how you add a new *.next* event onto a subject, passing the element as the parameter. 

Subjects can be used to bridge the gap between imperative and reactive programming, while observables are the basic building blocks of reactive programming in RxSwift.


### PublishSubjects
Starts empty and only emits new elements to subscribers.

Publish subjects let  subscribers to be notified of new events from the point at which they subscribed, until they either unsubscribe, or the subject has terminated with a .completed or .error event.

```swift
print(#function)
    let subject = PublishSubject<String>()
    let subscriptionOne = subject.subscribe(onNext: { event in
      print(event)
    })
    
    subject.onNext("1")
    subject.onNext("2")
    
    let subscriptionTwo = subject.subscribe { event in
      print("Subscription 2 ", event.element ?? event)
    }
    
    subject.onNext("3")
    
    subscriptionOne.dispose()
    
    subject.onNext("4")
    
    subject.onCompleted()
    
    subject.onNext("5")
    
    subscriptionTwo.dispose()
    
    let disposeBag = DisposeBag()
    let subscriptionThree = subject.subscribe {
      print("Subscription 3", $0.element ?? $0)
    }.disposed(by: disposeBag)
    
    subject.onNext("6")

   // Console
   // testPublisher()
   // 1
   // 2
   // 3
   // Subscription 2  3
   // Subscription 2  4
   // Subscription 2  completed
   // Subscription 3 completed
```

Subjects, once terminated, will re-emit its stop event to future subscribers. 

### BehaviorSubject
Starts with an initial value and replays it or the latest element to new subscribers.
Sometimes you want to let new subscribers know what was the latest emitted element, even though that element was emitted before the subscription. 

Since BehaviorSubject always emits the latest element, you can’t create one without providing an initial value. If you can’t provide an initial value at creation time, that probably means you need to use a PublishSubject instead.


```swift
    let subject = BehaviorSubject(value: "Initial value")
    let disposeBag = DisposeBag()
    
    subject.subscribe {
      print("1-", ($0.element ?? $0.error) ?? $0)
    }.disposed(by: disposeBag)
    
    subject.onError(MyError.anError)
    
    subject.subscribe {
      print("2-", ($0.element ?? $0.error) ?? $0)
    }.disposed(by: disposeBag)
    
    subject.onNext("3333")

    // Output 
    // 1-Initial value
    // 1-anError
    // 2-anError
```

Behavior subjects are useful when you want to pre-populate a view with the most recent data.

### ReplaySubject
Initialized with a buffer size and will maintain a buffer of elements up to that size and replay it to new subscribers.
if you wanted to show more than the latest value, we could the ReplaySubject to do it.

Replay subjects will temporarily cache, or buffer, the latest elements they emit, up to a specified size of your choosing. They will then replay that buffer to new subscribers.

```swift
let buffer = 2
let subject = ReplaySubject<String>.create(bufferSize: buffer)
let disposeBag = DisposeBag()

subject.onNext("1")
subject.onNext("2")
subject.onNext("3")

subject.subscribe {
  print("1-", ($0.element ?? $0.error) ?? $0)
}.disposed(by: disposeBag)

subject.subscribe {
  print("2-", ($0.element ?? $0.error) ?? $0)
}.disposed(by: disposeBag)

subject.onNext("4")

subject.onError(MyError.anError)

subject.subscribe {
  print("3-", ($0.element ?? $0.error) ?? $0)
}.disposed(by: disposeBag)

// Output
// 1- 2
// 1- 3
// 2- 2
// 2- 3
// 1- 4
// 2- 4
// 1- anError
// 2- anError
// 3- 3
// 3- 4
// 3- anError
```

### AsyncSubject
Emits **only** the *last* .next event in the sequence, and only when the subject receives a .completed event. 


### Relay
These wrap their relative subjects, but only accept .next events. You cannot add a .completed or .error event onto relays at all, so they're great for non-terminating sequences.
A relay wraps a subject while maintaining its replay behavior.

Unlike other subjects you add a value onto a relay by using the accept(_:) method. In other words, you don’t use onNext(_:).


#### PublishRelay

A PublishRelay wraps a PublishSubject and a BehaviorRelay wraps a BehaviorSubject. What sets relays apart from their wrapped subjects is that they are guaranteed to never terminate.

```swift
import RxRelay

let relay = PublishRelay<String>()
let disposeBag = DisposeBag()

relay.accept("First try")

relay.subscribe(onNext: {
  print($0)
}).disposed(by: disposeBag)

relay.accept("Second try")
```

> Note: Publish relays simply wrap a publish subject and work just like them, except the accept part and that they will not terminate. 


### BehaviorRelay
Behavior relays also will not terminate with a `.completed` or `.error` event. Because it wraps a behavior subject, a behavior relay is created with an initial value, and it will replay its latest or initial value to new subscribers. i
With behavior relay you can check for the current value without the need to subscribe to it.

```swift
import RxRelay

let relay = BehaviorRelay<String>(value: "Inital value")
let disposeBag = DisposeBag()

relay.accept("New initial value")

relay.subscribe(onNext: {
  print("1-", $0)
}).disposed(by: disposeBag)

relay.accept("1")

relay.subscribe(onNext: {
  print("2-", $0)
}).disposed(by: disposeBag)

relay.accept("2")

print("Relay current value is: ", relay.value)
```



