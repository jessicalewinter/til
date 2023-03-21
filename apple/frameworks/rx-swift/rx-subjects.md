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


### ReplaySubject
Initialized with a buffer size and will maintain a buffer of elements up to that size and replay it to new subscribers.

### AsyncSubject
Emits **only** the *last* .next event in the sequence, and only when the subject receives a .completed event. 


### Relay
These wrap their relative subjects, but only accept .next events. You cannot add a .completed or .error event onto relays at all, so they're great for non-terminating sequences.

#### PublishRelay


### BehaviorRelay


