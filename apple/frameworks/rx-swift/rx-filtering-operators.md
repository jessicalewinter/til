# Filtering Operators

## Intro
RxSwift’s filtering operators can be used to apply conditional constraints to .next events, that way the subscriber only receives the elements it wants to deal with.

## Operators

### ignoreElements
**ignoreElements** will do that: ignore `.next` event elements. It will, however, allow stop events through, such as `.completed` or `.error` events.

```swift
let disposeBag = DisposeBag()
let subject = PublishSubject<String>()


subject
  .ignoreElements()
  .subscribe { _ in
    print("You're out")
  }.disposed(by: disposeBag)

subject.onNext("X")
subject.onCompleted()
subject.onNext("X")
```

### element(at:)
The *elementAt* it used when you want to handle only a specific index emited element, it takes the index of the element you want to receive, and it ignores everything else.

```swift
let disposeBag = DisposeBag()
let subject = PublishSubject<String>()

subject
  .element(at: 2)
  .subscribe(onNext: { event in
    print("The only event is going to be emmited is \(event)")
  })
  .disposed(by: disposeBag)

for i in 1..<4 {
  subject.onNext("Element \(i)")
}
```

As soon as an element is emitted at the provided index, the subscription will be terminated.

### filter
When you need to do specific filtering, you can use the functional filter function. You add your condition, and the filter funtion will only allow those elements for which the predicatee resolves to true.

Ex:
```swift
let disposeBag = DisposeBag()
let observable = Observable.of(1, 2, 3, 4, 5, 6)

observable
  .skip(2)
  .subscribe(onNext: {
    print($0)
  }).disposed(by: disposeBag)

// Output
// 2, 4, 6
```

### skip
If you want to skip a specific amount of elemetns, you can use the skip function to accomplish that. It ignores from the 1st to the number you pass as its parameter.

```swift
let disposeBag = DisposeBag()
let observable = Observable.of(1, 2, 3, 4, 5, 6)

observable
  .skip(2)
  .subscribe(onNext: {
    print($0)
  }).disposed(by: disposeBag)

// Output
// 3, 4, 5, 6
```

### skip(while:)
`skip(while:)` will only skip up until something is not skipped, and then it will let continue from that point.

```swift
let disposeBag = DisposeBag()
let observable = Observable.of(2, 2, 3, 4, 4, 4)

observable
  .skip(while: { $0 % 2 == 0 })
  .subscribe(onNext: {
    print($0)
  }).disposed(by: disposeBag)

// Output
// 3, 4, 4, 4
```

### skip(until:)
If you want to filter elements until another observable emits .next event, then you should use skip(until:), which will keep skipping elements from the source observable (the one you’re subscribing to) until some other trigger observable emits.

```swift
let disposeBag = DisposeBag()
let subject = PublishSubject<String>()
let trigger = PublishSubject<String>()

subject
  .skip(until: trigger)
  .subscribe(onNext: {
    print($0)
  }).disposed(by: disposeBag)

subject.onNext("A")
subject.onNext("B")

trigger.onNext("Test")

subject.onNext("C")

// Output
// C
```

### take
If you want to take the first numbers of a certain sequence, you can use take to accomplish that

```swift

let disposeBag = DisposeBag()
let observable = Observable.of(1, 2, 3, 4, 5, 6)

observable
  .take(3)
  .subscribe(onNext: {
  print($0)
}).disposed(by: disposeBag)

// Output
// 1, 2, 3
```

### take(while:)
take(while:) works similar to skip(while:), but instead of skipping you're taking values. You can use the enumerated opeerator to do operations with the index of the emmited sequence.

```swift
let disposeBag = DisposeBag()
let observable = Observable.of(2, 2, 1, 4, 5, 6)

observable
  .enumerated()
  .take(while: { index, number in
    number % 2 == 0 && index < 3
  })
  .map { $0.element }
  .subscribe(onNext: {
    print($0)
  }).disposed(by: disposeBag)

// Output
// 2, 2
```

### take(until:)
Just like skip(until:), but after a trigger observable emits its first emmited onNext event, it will emit  all the events until that point

```swift
let disposeBag = DisposeBag()
let subject = PublishSubject<String>()
let trigger = PublishSubject<String>()

subject
  .take(until: trigger)
  .subscribe(onNext: {
    print($0)
  }).disposed(by: disposeBag)

subject.onNext("A")
subject.onNext("B")

trigger.onNext("Test")

subject.onNext("C")

// Output
// A, B
```

### distinctUntilChanged
`distinctUntilChanged` operator emits only unique values from an observable sequence

```swift
let disposeBag = DisposeBag()
let observable = Observable.of(1, 2, 2, 3, 3, 2)

observable
  .distinctUntilChanged()
  .subscribe(onNext: {
    print($0)
  }).disposed(by: disposeBag)

// Output
// 1, 2, 3, 2 
```
distinctUntilChanged only avoid duplicates that are in sequence, in the example above the 2 is printed out becasuse is comes after a different number(3)

You can use the comparator on distinctUntilChanged to add your own comparation rules

