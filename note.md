# Getting Started RxSwift Observables and Observer
## Let's try to make it nice and easy


### Motivation
  - Goal is to describe three event types that `observables` can throw.
  - Think about your phone.
  - Observer is "you". You are using the phone waiting for the next notification.
  - `subscribe` is analogous to

### Recap
  - To make reactive, you need to send a stream of data/events
  - So that you may "observe" the change.

### Observables
  - Sequence protocol with Iterator Protocol
  - Observers are built-in internally with `Observables`.
  -

### Observer
  - The closure block is executed when you "subscribe" to see the change.

### Subscribe
  -

### Application
  -


### Last Remarks
  - Join the mailing list if you haven't here. Typeform


#### Type
```swift
  enum Event<Element>  {
      case next(Element)      // next element of a sequence
      case error(Swift.Error) // sequence failed with error
      case completed          // sequence terminated successfully
  }

  class Observable<Element> {
      func subscribe(_ observer: Observer<Element>) -> Disposable
  }

  protocol ObserverType {
      func on(_ event: Event<Element>)
  }
```


#### Relationship
```swift
func myFrom<E>(_ sequence: [E]) -> Observable<E> {
    return Observable.create { observer in
        for element in sequence {
            observer.on(.next(element))
        }

        observer.on(.completed)
        return Disposables.create()
    }
}

let stringCounter = myFrom(["first", "second"])

print("Started ----")

// first time
stringCounter
    .subscribe(onNext: { n in
        print(n)
    })

print("----")

// again
stringCounter
    .subscribe(onNext: { n in
        print(n)
    })
```
