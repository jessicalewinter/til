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

