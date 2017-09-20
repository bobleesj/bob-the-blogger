# Getting Started RxSwift with Bob
## Why Learn Reactive Programming with RxSwift

### Motivation
I'd like to begin this article by sharing my frustration with a pool of misleading and often "this is how it just works" articles floating around the internet. I'm tired of seeing myself getting tired after having read through the first 10 pages of the Google search.

I've conducted over 1000+ survey results from the iOS developers and students. `RxSwift` has been brought up often times. Yet, most of them have a hard time using this framework into practice for there is a steep learning curve.

Now I see why. There aren't many people who can explain the concept using analogy and stories. You've come to the right place. I've decided to get my hands dirty with `RxSwift` and contribute to this phenomenal community.

### Learning Mentality
I'm not a big fan of getting started just by "doing things". My learning approach has been always getting the fundamental root of "why" rather than "how". Most people tend to focus on building products. Yes, it may take more investment and effort in the beginning. Down the road, however, you code with confidence and conviction rather than insecurity.

Yes, you can learn "by doing it". You can learn that you can't crack a rock with an egg by physically throwing an egg. On the flip side, you can also watch YouTube videos somebody throwing an egg at the rock.

Yes, you can't learn to ride a bicycle by just reading books about aerodynamics and  angular momentum. In fact, most don't need to about those subjects in order to use it as a tool.

There must a balance of both. For my learning, I spend the majority of my time looking through the theories and pre-made examples before getting started. I don't attempt to make my project until I can explain why we use the particular paradigm or architecture to my 6 year-old kid.

### Questioning Why
In this article, you will get a strong understanding of why we use `RxSwift` and why you should bother listening to what it has to offer.

My favorite quote from "Man's search for meaning" by Viktor Frankl goes,

> Those who have a 'why' to live, can bear with almost any 'how'.”

The author was able to survive through the Nazis' concentration camps. As a doctor, he had the reason to live. First,  to see his family alive. Second, to help the community. The same mentality applies to learning. If you find out "why" you use `RxSwift`, you will eventually find out how to apply the concept in various real world circumstances via a balance of pre-made examples and imagination.

Einstein's quote goes,

> "Imagination is more important than knowledge. For knowledge is limited, whereas imagination embraces the entire world, stimulating progress, giving birth to evolution."


### Audience
This article is written for advanced developers. If you attempt to learn functional/reactive programming without having a firm understanding of `OOP`, `POP`, `ARC`, `delegate`, `callbacks`, `functional programming`, you will not be able to understand the rest. If you want to get everything in one place. Feel free to enroll this course called, "Learn Swift 4 with Bob". Perhaps, this is the prerequisite of `RxSwift`.

<iframe width=100% height="315" src="https://www.youtube.com/embed/7lwgI2I-vKU" frameborder="0" allowfullscreen alt="Prerequisite"></iframe>

### Definition of RxSwift
Most tutorials begin by describing `RxSwift`  as a tool that allows developers to code with reactive and functional paradigm. However, most of them fail to explain what each of the terms, `reactive` and `functional` stands for and why we adopt its philosophy. They fail to explain the benefits over the traditional. They are just busy "showing examples" - how, rather, the "why'".

> **Note:** `RxSwift` is just a library written in Swift. Other languages support `RxJS`, `RxKotlin`, `RXScala`, `Rx.NET`. They all share the same paradigm curated by [ReactiveX](http://reactivex.io/).

Let us find out what `reactive` and `functional` offer on the table before diving into `RxSwift`.

### What is Functional Programming
The first principle `RxSwift` adapts is function-driven coding style. In fact, I've already covered in [Intro to Swift Functional Programming with Bob](https://www.bobthedeveloper.io/blog/intro-to-swift-functional-programming-with-bob). Just to repeat myself, I will go over a few points.

> **Definition:** Functional programming is nothing more than using functions to communicate with your co-workers and solve problems.

To appreciate functional programming, let's begin with a "normal" way of solving problems.

#### Imperative
Let say we have an array of elements.

```swift
let myGrade = ["A", "A", "A", "A", "B", "D"]
```

You only want to get "A"s. Let's begin imperatively.

```swift
var happyGrade: [String] = []

for grade in recentGrade {
 if grade == "A" {
  happyGrade.append(grade)
 } else {
  print("My mama ain't happy")
 }
}

print(happyGrade) // ["A", "A", "A", "A"]
```

#### Declarative.
The above implementation is hideous. 8 lines of code. Let's use a function to become more functional.

```swift
stringFilter(grade: myGrade, returnBool: { $0 == “A” })
// ["A", "A", "A", "A"]
```

> **Note:** If you have a hard time understanding the difference in the meaning of imperative vs declarative, feel free to take a look at the attached article/course above and come back after. Again, you need to have a strong understanding of closures and completion handlers.

Functional programming offers:
  1. No explicit state management
  2.  Streamlined communication through conventional parameter naming due to  `$`
  3.  Unit-testability
  4.  Short

> **Note:** I've already covered each of them benefits above in the previous article.

### What is Reactive Programming
The second paradigm `RxSwift` adopts is reactive programming  To appreciate, let us take a look at "non-reactive" code to appreciate reactive code.

#### Non-Reactive
You want to add two numbers.

```swift
var a = 1
var b = 3

a + b // 4
```

The result is `4`. Nice and easy. But, what if you want to change `a` to a different number?

```swift
var a = 1
var b = 3

a + b  // Initial(4) -> Final(4)

a = 3
```

Now, there isn't any difference. Even if you change the assigned value of `a` from `1` to `3`, `a +  b` still returns `4`. This is just a "normal" code in Swift.  `a + b` doesn't care anything that happens. It only minds its own business at the particular time.

#### Reactive
Now, let's find out what reactive programming stands for. Let us use the identical example. However, we will try to wrap those numbers using a made-up class called `Reactive`.

```swift
var a = Reactive(1)
var b = Reactive(3)

a.value + b.value // 4
```

> **Note:** I've used a fake class called `Reactive` since the default value types, such as `String`, are non-reactive in Swift as you've seen in the example above.

Now, let us mutate the value of `a` to 3.

```swift
var a = Reactive(1)
var b = Reactive(3)

a.value + b.value // Initial(4) -> Final(6)

a.value = 3
```

Now, there is a difference. `a.value + b.value` still cares about the value of `a` and `b`. This is what reactive programming is. **It waits for changes and then indicates/reacts/shows/expresses/subscribes/observes.** It's similar to `NSNotification`.

This is the fundamental basis of reactive programming.

### Imagination
Let us take a moment and imagine how powerful this reactive paradigm can be in real-world iOS programming.

Due to the nature of reactiveness, `RxSwift` is king of handling asynchronous code because every time you make a change, it will tell you that something has happened.

For example, If you make properties of `UIView`, `UIDevice` reactive, you can easily listen to changes and execute other lines of code via completion handlers/callbacks. Let's begin by making the device orientation property reactive.  

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

Now, whenever the user flips, 90 degrees 4 times,

```swift
// "Landscape
// "Weird Portrait"
// "Landscape"
// "Portrait"
```

it's pretty easy. That's it. That is the fundamental basis of `Reactive Programming`.

#### Functional Programming
But, how does `functional` programming play a role? Well, you can apply "function" to manipulate the events you've received from the `reactive` objects.

Let us say you only want to `print` when the device is at a `portrait` mode.

```swift
reactiveOrientation
.filter { $0 == .portrait }
.observe { currentOrientation in
  switch currentOrientation {
  case .landscapeRight: print("Landscape!")
  case .portraitUpsideDown: print("Weird portrait!")
  case .landscapeLeft: print("Landscape!")
  case .portrait { print("Portrait!") }
  }
```

Or since you've already mastered `closures` in Swift, you can also express this way,

```swift
reactiveOrientation
.filter { $0 == .portrait } // if false, return
.observe {  
  switch $0 {
  case .landscapeRight: print("Landscape!")
  case .portraitUpsideDown: print("Weird portrait!")
  case .landscapeLeft: print("Landscape!")
  case .portrait { print("Portrait!") }
  }
```

Let's rotate your phone 90 degrees in a clockwise.

```swift
// x printed
// x printed
// x printed
// "Portrait!"
```

Again, the `filter` function is applied to each incoming data/event to manipulate what you get. The functional/reactive paradigm helps you write fewer lines of code and improves readability. To see whether the previous statement is true, let's begin by looking at a couple other examples to handle async code.

> If you are not familiar with asynchronous code in Swift, feel free to read [The delegate and Callbacks in iOS](https://www.bobthedeveloper.io/blog/the-delegate-and-callbacks-in-ios). Again, asynchronous can be described as irregular streams of events/execution


#### Comparison with Delegate
No more bloated `UIViewController` tried of confronting to a dozen protocols.

```swift
extension MYViewController: UIViewController, GoogleMapDelegate, GoogleMapDataSource, UIDeviceOrientationDelegate, UITableViewDelegate, AnotherDelegate {
  // A dozen of required methods
  // Hard to navigate
}
```

No more using `objc` to have "optional" required functions

```swift
@objc protocol UIDeviceOrientationDelegate {
    @objc optional func didChangeDeviceOrientation()
}
```

A couple is great, but more can be rough.

#### Comparison with KVO
Good old Objective-C developers often use `KVO` to track any changes to the particular object.

```swift
-(void)observeValueForKeyPath:(NSString *)keyPath
                     ofObject:(id)object
                       change:(NSDictionary *)change
                      context:(void *)context
```


You don't need to understand the code above. You may observe the property by defining "keypath". No worry. As a Swift developer, you rarely have to use this API explicitly.

#### Comparison with NSNotification
To subscribe, observer, it requires 4 parameters. While `RxSwift` only requires one method called, `subscribe`. No functional paradigm is applied.

```swift
NotificationCenter.default.addObserver(self,
    selector: #selector(doThisWhenNotify),
    name: NSNotification.Name(rawValue: myNotificationKey),
    object: nil)
  }
```


#### Comparison with Property Observer
You may also use `property observer`.

```swift
var myDeviceCurrentOrientation = UIDevice().current {
  willSet(newOrientation) {
    newOrientation in
      switch newOrientation {
      case .landscapeRight: print("Landscape!")
      case .portraitUpsideDown: print("Weird portrait!")
      case .landscapeLeft: print("Landscape!")
      case .portrait { print("Portrait!") }
      }
}
```

`Property Observers` provide a powerful way to handle code that has been mutated. It, however, contains shortcomings. First, if two more objects have to care about the new data from, you probably have done something like this,

```swift
var myDeviceCurrentOrientation = UIDevice().current {
  willSet(newOrientation) {
    // functionOne(newOrientation)
    // functionTwo(newOrientation)
    // functionThree(newOrientation)
  }
}
```

The property observer area becomes bloated. Using `RxSwift`, you can split.

```swift
myDeviceCurrentOrientation = UIDevice().current
let reactiveOriented = Reactive(myDeviceCurrentOrientation)
```

```swift
// functionOne
func functionOne() {
  reactiveOriented.observe { print($0) }
}

// functionTwo
func functionTwo() {
  reactiveOriented.observe { print($0) }
}

// functionThree
func functionTwo() {
  reactiveOriented.observe { print($0) }
}
```

Now, your code becomes more modularized.


### Last Remarks
The purpose of this piece wasn't to get you started building products with `RxSwift`. Rather, it was used to make sure you understand the fundamental building blocks of `RxSwift`. Reactive programming is a whole new ball game. In the future, I plan to continue to go in-detail with `RxSwift`. If you are interested in getting future updates, please feel free to email me at `bob@bobthedeveloper`.
