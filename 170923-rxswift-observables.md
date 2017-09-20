# Learn RxSwift Observables and Observer with Bob
## Let's try to make it nice and easy

### Motivation
Great to see you back lovely readers. In the last post, 'Getting Started with RxSwift', I've talked about the reason why developers might consider adopting its philosophy. Today, let's make it real.

### Prerequisite
A complete understanding of closures, memory management, `Sequence` and `IteratorProtocol`. If you are not comfortable with any of these topics, I've covered them all.  Feel free to join `Learn Swift 4 with Bob: The intermediate to Advanced` [here](https://www.udemy.com/learn-swift-with-bob/?couponCode=BOBTHEDEVELOPER).

### Recap
In the previous post, I described the basis of reactive programming by creating a artificial class called, `Reactive`.

```swift
let deviceOrientation = UIDevice.current
let reactiveOrientation = Reactive(deviceOrientation)
```

You've made `deviceOrientation` "reactive". Whenever the user rotates the phone, you now can "observe" the change as shown by the pseudo-code below

```swift
reactiveOrientation.observe { currentOrientation in
  switch currentOrientation {
  case .landscapeRight: print("Landscape!")
  case .portraitUpsideDown: print("Weird portrait!")
  case .landscapeLeft: print("Landscape!")
  case .portrait { print("Portrait!") }
  }
}
```

And then it starts pumping out those event when you rotate.

```swift
// "Landscape
// "Weird Portrait"
// "Landscape"
// "Portrait"
```




 - Emitter
 - Receiver
