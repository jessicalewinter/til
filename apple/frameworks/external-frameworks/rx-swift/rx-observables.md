# Observables

You came from a different reactive framework, you'll see a lot of different names for Observable. 
"observable", "observable sequence", "sequence" used interchangeably in Rx. "Stream" is the same as "sequence", but in RxSwift we use "sequence".


## Lifecycle of an observable
When an observable emits an element, we called it a `next` element

When an observable emits a finite total of events and then it ends, er called it a completed event, because it has been terminated.

If an observable emits an `error` event, just as with the completed event, it will be terminated and will not emit any event further.



- An observable emits next events that contain elements of a type
- It will continue to do this until a terminating event is emitted
- Once an observable is terminated it can no longer emit events.


### Operators

### just
Just creates an observable sequence containing just a single element

```swift
let observable = Observable<Int>.just(1)
let observable2 = Observable<[Int]>.just([1, 2, 3])

```

### of
of receives a variadic sequence of elements

```swift
let observable2 = Observable.of(one, two, three)
// observable2 is of type Int, not an array
```

if you want to create an observable array, you can simply pass an array to of


```swift
let observable3 = Observable.of([one, two, three])
// observable3 is of type [Int]
```


### from
The from operator creates an observable of individual elements from an array of typed elements.The from operator only takes an array.

```swift
  let observable4 = Observable.from([one, two, three])
// observable4 is of type Int not [Int]
``` 

## Subscribing
An observable won’t send events, or perform any work, until it has a
subscriber.

```swift
let observable = Observable.of(one, two, three)
observable.subscribe { event in
    print(event)
}
// next(1)
// next(2)
// next(3)
// completed

```

The observable emits a .next event for each element, then emits a .completed event and finally is terminated.


### empty
The empty operator creates an empty observable sequence with zero elements; it will only emit a .completed event.

```swift
    let observable = Observable<Void>.empty()
    observable.subscribe(onNext: { element in
      print(element)
    }, onCompleted: {
      print("completed")
    })

```

*Empty* observables are handy empty observable? Well, they’re handy when you want to return an observable that immediately terminates or intentionally has zero values.


### never
The *never* operator creates an observable that doesn’t emit anything and never terminates.

```swift
let observableNever = Observable<Any>.never()
print("never example")
observableNever.subscribe { element in
  print("My nem element is: \(element)")
} onError: { error in
  print("my error is: \(error)")
} onCompleted: {
  print("Completed")
} onDisposed: {
  print("Disposed")
}

```

### range
You can crete an observable from a range of values, which takes a start integer value and a count of sequential integers to generate.

```swift
let observableRange = Observable<Int>.range(start: 1, count: 10)
print("range example")
observableRange.subscribe { element in
    let n = Double(element)
    let fibonacci = Int(((pow(1.61803, n) - pow(0.61803, n)) / 2.23606).rounded())
    print(fibonacci)
} onError: { error in
    print("my error is: \(error)")
} onCompleted: {
    print("Completed")
} onDisposed: {
    print("Disposed")
}

```


## Disposables
A dispose bag holds disposables — typically added using
the .disposed(by:) method — and will call dispose() on each one when the dispose bag is about to be deallocated.

```swift
let observableTest = Observable.of("A", "B", "C")
let disposeBag = DisposeBag()
observableTest
    .subscribe { print($0.element)}
    .disposed(by: disposeBag)
```

If you forget to add a subscription to a dispose bag, if you dispose it manually, or in some other way cause the observable to terminate at some point, you will probably leak memory.


#### create
Using the create operator is another way to specify all events that an observable will emit to subscribers, instead of need to  created observables with specific .next event elements.

```swift
let disposeBag = DisposeBag()
Observable<String>.create { observer in
    observer.onNext("1")
    observer.onCompleted()
    observer.onNext("2")
    
    return Disposables.create()
}.subscribe(
    onNext: { print($0) },
    onError: { print($0) },
    onCompleted: { print("Completed") },
    onDisposed: { print("Disposed") }
).disposed(by: disposeBag)
```

The create operator takes a single parameter named subscribe. Its job is to provide the implementation of calling subscribe on the observable.


## Observable factories(aka deferred)
Rather than creating an observable that waits around for subscribers, it’s possible to create observable factories that vend a new observable to each subscriber.

```swift
let disposeBag = DisposeBag()
var flip: Bool = false

let factory = Observable.deferred {
    flip.toggle()
    return flip ? Observable.of(1, 2, 3) : Observable.of(4, 5, 6)
}

for _ in 1...4 {
    factory.subscribe(onNext: {
        print($0, terminator: "")
    }).disposed(by: disposeBag)
    print()
}

```

## Traits
Traits are observables with a narrower set of behaviors than regular observables.
There are three kinds of traits in RxSwift: *Single*, *Maybe* and *Completable*

#### Single
Single will emit either a .success(value) or .error event. *.success(value)* is actually a combination of the .next and .completed events. 

```swift
Single.create { single in
  let disposable = Disposables.create()
            
  guard let path = Bundle.main.path(forResource: name, ofType: extensionPath) else {
     single(.failure(FileReaderError.fileNotFound))
        return disposable
  }

  single(.success(path))
  return disposable
}
.subscribe {
    switch $0 {
    case .success(let content):
        print(content)
    case .failure(let error):
        print(error)
    }
} 
```

This kind of trait is useful in situations such as saving a file, downloading a file, loading data from disk or basically any asynchronous operation that yields a value. 
To better express your intention to consume a single element from a sequence and ensure if the sequence emits more than one element the subscription will error out. To achieve it, you can subscribe to any observable and use `.asSingle()` to convert it to a `Single`.

#### Completable
A Completable will only emit a .completed or .error event. It doesn't emit any values.
This variation of Observable allows only for a single .completed or .error event to be emitted before the subscription is disposed of.
You can convert an observable sequence to a completable by using the ignoreElements() operator, in which case all next events will be ignored, with only a completed or error event emitted, just as required for a Completable.

```swift
saveDocument()
  .andThen(Observable.from(createMes))
  .subscribe(onNext: { message in
    message.display()
  }, onError: {e in
    alert(e.localizedDescription)
  })
```
> Note: 
The `andThen` operator allows you to chain more completables or observables upon a success event and subscribe for the final result. 


#### Maybe
Maybe is a merge of a Single and Completable. It can either emit a .success(value), .completed or .error. 

Maybe is quite similar to Single with the only difference that the observable may not emit a value upon successful completion.


Just as with Single, you can either create a Maybe directly by using `Maybe.create({ ... })` or by converting any observable sequence via `.asMaybe()`.
