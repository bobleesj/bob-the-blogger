# Getting Started RxSwift with Bob
## Why Learn Reactive Programming with RxSwift

### Motivation
I'd like to begin this article by sharing my frustration with a pool of misleading and often "this is how it just works" articles floating around the internet. I'm tired of seeing myself getting tired after having read through the first 10 pages of Google Search.

I've conducted over 1000+ survey results from the iOS developers and students. `RxSwift` has been the number one interest. Yet, there is a steep learning curve. Now I see why. There aren't many people who can explain the concept using analogy and story. You've come to the right place. I've decided to get my hands dirty with RxSwift and contribute to this community.

### Learning Mentality
I'm not a big fan of getting started just by "doing things". My learning approach has been always getting the fundamental root of "why" rather than "how". Most people tend to focus on building products. Yes, it does take a bit longer time in the beginning. Down the road, however, I code with confidence and conviction rather than insecurity.

Yes, you can learn "by doing it". You can learn that you can't crack a rock with an egg. One can learn through physically throwing an egg, but you can also watch videos someone else throwing an egg.

Yes, you can't learn to ride a bicycle by just studying the theory. It's a balance of both. However, when it comes to "learning" in forms of knowledge, rather than physical skills, the riding the bicycle analogy does not apply as much.

 I spend a majority of my time looking through the theories and examples before getting started. I just don't get started until I can explain why we use the particular paradigm or architecture to my 6 year-old kid. I look at many examples and theories, I just don't make my own till later.

### Fundamentals
In this article, you will get a strong understanding of why we use `RxSwift` and why you should bother listen to what it has to offer.

My favorite quote from "Man's search for meaning" by Viktor Frankl goes,

> Those who have a 'why' to live, can bear with almost any 'how'.”

The author was able to survive through concentration camps. As a doctor, he had the reason to live. First was to see his family alive. Second, to help the community  The mentality applies to learning anything. If you find out "why" you use `RxSwift`, you will eventually find out how to apply the concept to real world programing through a balance between pre-made examples and imagination

Einstein's quote goes,

> "Imagination is more important than knowledge. For knowledge is limited, whereas imagination embraces the entire world, stimulating progress, giving birth to evolution."


### Audience
This article is written for advanced developers. If you attempt to learn functional/reactive programming without having a firm understanding of `OOP`, `POP`, `ARC`, `delegate`, `callbacks`, `functional programming`, you will not be able to understand the rest. Please visit the blog. If you want to get everything in one place. Feel free to enroll this $50 course called, "Learn Swift 4 with Bob". In fact, from now on, this is the prerequisite of RxSwift.

<iframe width=100% height="315" src="https://www.youtube.com/embed/7lwgI2I-vKU" frameborder="0" allowfullscreen alt="Prerequisite"></iframe>

### Definition of RxSwift
Most tutorials begin by saying `RxSwift` is often referred to as a tool that allows developers to code with reactive and functional paradigm. However, most of them fail to explain what each of the terms, `reactive` and `functional` stand for the development of software. They fail to explain the benefits over the traditional. They are just busy "showing examples" - the how, rather than, "why'".

> **Note:** 'RxSwift' is just a library written in Swift. Other languages support RxJS, `RxKotlin`, `RXScala`, `Rx.NET`. They all share the same paradigm curated by [ReactiveX](http://reactivex.io/).

### What is Functional Programming
First first principle `RxSwift` adapts is function-driven coding style. In fact, I've already covered in [Intro to Swift Functional Programming with Bob](https://www.bobthedeveloper.io/blog/intro-to-swift-functional-programming-with-bob). But, I will go through just a couple points.

Functional programming is nothing more than using functions to communicate between your co-workers and solve problems.

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
  3.  Super easy to unit-test
  4.  Short

  > **Note:** I've already covered each of them benefit above in the previous article.


### What is Reactive Programming
Great, we've taken a look at what functional programming offers. What does reactive programming offer? Well, first of all, let's take a look at "non-reactive" code to appreciate reactive code.

#### Non-Reactive
Let us you want to add two numbers.

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

Now, there isn't any difference. Even if you change the assigned value of `a` from `1` to `3`, `a+b` still returns `4`. This is just a "normal" code in Swift.  `a+b` doesn't care anything that happens. It only minds its own business at the particular time.

#### Reactive
Now, let's find out what is reactive programming. Let us use the identical example above. However, we will try to wrap those numbers using a made-up class called `Reactive`.

```swift
var a = Reactive(1)
var b = Reactive(3)

a.value + b.value
```

> **Note:** I've used a fake class called `Reactive` since the default value types, such as `String`, are non-reactive in Swift as you've seen on the example above.

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
Great, now let's take a moment and imagine how powerful this paradigm can be in real-world iOS programming.

Due to the nature of reactiveness, `RxSwift` is king of handling asynchronous code because whenever you make a change, it will tell you that something has happened.

For example, If you make properties of `UIView`, `UIDevice` reactive, you can easily listen to the change and execute other lines of code using completion handlers/callbacks.

However, how does functional programming kicks in? You can use functions to manipulate the data you've received by observing. First let's see how it can used to detect a change in orientation of the phone.

```swift
let deviceOrientation = UIDevice.current
let reactiveOrientation = Reactive(deviceOrientation)
```

You've made `deviceOrientation` "reactive". Whenever the user rotates the phone, you now can "observe" the change as shown by the psudo code below

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

But, how does `functional` programming play a role? Well, you can apply "function" to the event you've received from the `reactive` variables.

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

Again, functional programming is applied to each incoming data.

 it will help you write fewer lines of code and improves readability compared to other ways. Let's now begin by looking at a couple examples.

> If you are not familiar with asynchronous code in Swift, feel free to read [The delegate and Callbacks in iOS](https://www.bobthedeveloper.io/blog/the-delegate-and-callbacks-in-ios). Again, asynchronous can be described as irregular streams of events/execution.

### RxSwift
Again, there are many ways to handle async code in iOS. One of the most popular ones since ``

I can't talk about all the pros and cons of each since it would be another article. But, I will just show you with code.
#### Comparison with Delegate
No more using `objc` to have "optional" required functions

```swift
@objc protocol UIDeviceOrientationDelegate {
    @objc optional func didChangeDeviceOrientation()
}
```

No more bloated `UIViewController` tried of confronting to a dozen protocols.

```swift
extension MYViewController: UIViewController, GoogleMapDelegate, GoogleMapDataSource, UIDeviceOrientationDelegate, UITableViewDelegate
```

A couple is great, but more can be rough.

#### Comparison with KVO
Good old Objective-C developers often use this API to track whether anything has happened to the property.


```swift
-(void)observeValueForKeyPath:(NSString *)keyPath
                     ofObject:(id)object
                       change:(NSDictionary *)change
                      context:(void *)context
```

You don't need to understand the code above. But, you are simply "observing" the property by defining "keypath". No worry.

#### Others,
You may also use `property observer` in Swift

```swift
var myDeviceCurrentOrientation = UIDevice().current {
  willSet(newOrientation) {
    currentOrientation in
      switch currentOrientation {
      case .landscapeRight: print("Landscape!")
      case .portraitUpsideDown: print("Weird portrait!")
      case .landscapeLeft: print("Landscape!")
      case .portrait { print("Portrait!") }
      }
}
```

But why if now you want to `filter` out? What if you want to apply other functions such as `map`? The code above doesn't flow compared with the `reactive` way. `RxSwift` provides other features that `property observer` doesn't provide such as executing code on main/background thread.

One at a time, you will learn with me.

### Last Remarks 
I've much talked about `RxSwift` such as `Observables`, `Subject`, and so on. If you want to get `RxSwift` down the road, feel free to join the mailing list to get updated in the next post.
