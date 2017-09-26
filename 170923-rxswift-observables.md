# Fundamentals of RxSwift Observables and Observer with Bob
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

You've made `deviceOrientation` "reactive". As the user rotates the phone, you now "observe" the change as shown by the pseudo-code below

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

And then it starts pumping out those event as you rotate.

```swift
// "Landscape
// "Weird Portrait"
// "Landscape"
// "Portrait"
```

### Relationship
The goal is to make sure we can replicate the behavior as shown above. However, we will not going to start off with focusing on building UIs. Let us find out what goes underneath. To be specific, let us find out how to make objects "reactive" and may emit "events"/"data" in `RxSwift` for real.

### Prerequisite
Understanding of `sequence` protocol and its required method
 `makeIterator()`. `POP`, `OOP`, a complete understanding of `closure` and `delegate`. This is for advanced Swift developers. Feel free to enroll the course and come back after.

<!-- <iframe width=100% height="315" src="https://www.youtube.com/embed/7lwgI2I-vKU" frameborder="0" allowfullscreen alt="Prerequisite"></iframe> -->


### Make Reactive
The first problem is, "how do you make an object reactive?" As I've used a fake wrapper class called, `Reactive`. In`RxSwift`, it comes along with a built-in class called, `Observable`.

Think about `Observable` as a YouTube channel. You can subscribe to the channel but you don't have to. When you subscribe, you get new content(data) from the channel (observable)

Let us create an `observable` object.

```swift
 let reactiveObject = Observable.of("1", "2", "3")
```

However, it does not "emit" data yet. It is just like YouTube channel. You will only get data when you "subscribe" to the channel.

In `RxSwift`, there is a built-in method called, `subscribe`.

Of course, there are more than one subscriber to the YouTube channel. Each subscriber receive new fresh content from the server.

Let's now, "subscribe" to `reactiveObject`.

```swift
// Subscriber One
reactiveObject.subscribe(onNext: { print($0) })
```

Now, it emits a stream of new events.

```swift
// print("1")
// print("2")
// print("3")
```

Let there be more than one subscriber to the YouTube channel.


```swift
// Subscriber One
reactiveObject.subscribe(onNext: { print("Sub 1: " +  $0) })

// Subscriber Two
reactiveObject.subscribe(onNext: { print("Sub 2: " + $0) })
```

Now, each subscriber receive new data/event from the channel.

```swift
// print("Sub 1: 1")
// print("Sub 1: 2")
// print("Sub 1: 3")

// print("Sub 2: 1")
// print("Sub 2: 2")
// print("Sub 2: 3")
```

> **Note:** I'm not going to talk about memory management yet for better understanding. Will definitely cover soon.


### The Behind Scene
What's happening up there? It seems like the data is begin thrown at the subscriber from the YouTube channel. However, you will notice that it is sending data one at a time. "1" and then "2", instead of everything at once.

The process of emitting event/data using `observable` is analogous to a `for-in` loop which compiles into to call the `makeIterator()`method. Again, if you have a hard time understanding the previous statement, feel free to enroll the course, "Learn Swift 4 with Bob".

### Another Way to Create Observable
However, you have a complete control of manipulating each stream of data and event you wish to emit by creating your own `observable`.

In the `reactiveObject` example above, you were able to emit event when subscribed by using a built-in method called, `of`. You can also customize which event to emit in a whole different manner. Let's find out.

### Create Observable
