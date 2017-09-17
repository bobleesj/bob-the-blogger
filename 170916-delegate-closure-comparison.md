# The Delegate and Callbacks in iOS
## Deal with async code. There is no black and white, but certainly shifting.

### Story of Mine
I used to believe the world of programming is a gentle place where code is executed from top to bottom.

> If you are already familiar with sync/async code, feel free to get to the main section.

```swift
print("Hello, world")
print("Life is great")
print("How are you?")
```

Not so long after, I've discovered that we do not live in an ideal world. We may have to execute functions only after another.

This situation often occurs when you work with UI tasks such as pressing a button to network with the server.

```swift
func likePost() {
  // increase "likes" by one
}
```

Let us create an `IBAction` which is responsible for calling the `likePost` method above.

```swift
class MyViewController: UIViewController {  
  @IBAction func didPressLikeButton(sender: UIButton) {
    likePost() // increase like in database/coredata
  }
}
```

One thing to note with the above implementation. `likePost()` is called irregularly whenever the user presses the button. The user can spam it or wait 30 minutes and then like the post. Let's take a look at the drawing below.

```swift
---tab---1s---tab---2s---tab---300s---tab--->
```

The above also can be also described as,

```swift
---tab---other-tasks---tab---other-tasks---tab--->
```

> **Analogy:** Asynchronous code is like a waiter in a restaurant. When the customer orders (presses the button), the waiter does not have to wait all day from the kitchen. Instead, the he/she can do other tasks such as cleaning the dishes and serving other tables (execute other lines of code meanwhile).

Well, you can learn more about async vs sync code in the articles below.

 - [Intro to Grand Dispatch Central in Swift with Bob](https://www.bobthedeveloper.io/blog/intro-to-grand-central-dispatch-in-swift-with-bob)
 - [UI and Network like a boss in Swift](https://www.bobthedeveloper.io/blog/ui-&-networking-like-a-boss-in-swift)

### Prerequisite
Before you begin, this article is written for **advanced developers** who are already familiar with closure syntax and how the delegate/data source pattern works. If you are not comfortable with any, feel free to check out the articles below and come back.

 - [The complete understand of delegate in Swift](https://www.bobthedeveloper.io/blog/the-complete-understanding-of-swift-delegate-and-data-source)
 - [Completion Handlers in Swift with Bob](https://www.bobthedeveloper.io/blog/completion-handlers-in-swift-with-bob)

Besides, you are aware of memory management with Automatic Referencing Count.  If you want to get everything in one spot, feel free to enroll [Learn Swift 4 with Bob](https://goo.gl/L68U2z). The below is just a promo video. Feel free to skip.

<!-- <iframe width=100% height="315" src="https://www.youtube.com/embed/7lwgI2I-vKU" frameborder="0" allowfullscreen></iframe> -->


### Delegate vs Closure
As the title indicates, in iOS programming and other platforms such as Android, front-end, there are usually two ways to deal with async code. 1. The closure way 2. The delegate way.

> **Note:** There are many other ways including `KVO`, `property observers`, `notification`,  `sequence`, `target-action`.

In this article, I will discuss the pros and cons and possibly help you decide which pattern to use. However, this article isn't about choosing one just because everyone else, including Apple engineers and other famous people, is doing it. Let's build our own perspectives through looking at multiple platforms.

### Delegate Way
The early Apple engineers love the delegate pattern to handle async tasks. Let's create one.

> Again, I'm not going to explain how the delegate pattern works. I've covered previously.

#### Design Protocol
First, we will use design a protocol which will be used as a type that `UIViewController` will conforms to.

> If you are not comfortable with the previous statement, please do not proceed.

I have used an example from [Why you shouldn’t use delegates in Swift](https://medium.cobeisfresh.com/why-you-shouldn-t-use-delegates-in-swift-7ef808a7f16b) by Marin. Thanks for the example. As the title indicates, however, the article has failed to create an argument in a larger context. I will.

```swift
protocol NetworkServiceDelegate {
    func didComplete(result: String)
}
```

#### Design Delegator/Sender

```swift
class NetworkService {
    var delegate: NetworkServiceDelegate?

    func fetchDataFromUrl(url: String) {
        API.request(.GET, url) { result in
            delegate?.didCompleteRequest(result)
        }
    }
}
```

The `fetchDataFromUrl` method will be executed by `NetworkService`. When the task has been completed, it will call the `didCompleteRequest` method from the delegate.

#### Design Delegate/Receiver

```swift
class MyViewController: UIViewController, NetworkServiceDelegate {

    let networkService = NetworkService()

    override func viewDidLoad() {
        super.viewDidLoad()
        networkService.delegate = self
    }

    func didCompleteRequest(result: String) {
        print("I got \(result) from the server!")
    }
}
```

Now, whenever the user press a button, and calls,

```swift
@IBAction func didPressButton(sender: UIButton) {
  networkService.fetchDataFromUrl("http://localhost:4000/user")
}
```

As mentioned above, the `didCompleteRequest` method is called by the delegator.

#### Retain Cycle
There is a strong reference cycle as `MYViewController` has a property of `NetworkService` and `NetworkService` contains a delegate property which has been assigned by `MYViewController`.  To prevent this from happening, you have to define the delegate as `weak` and add `class` to the protocol.

```swift
weak var delegate: NetworkServiceDelegate?
```

and

```swift
protocol NetworkServiceDelegate: class {
    func didComplete(result: String)
}
```

If you are not understanding the statement above, again, feel free to enroll the course and then come back after. This article is written for advanced developers.

### Closure Way
Now, let's compare with the closure/completion-handler/callback way.

#### Design Network Service

```swift
class NetworkService {

    var onComplete: ((result: String)->())?

    func fetchDataFromUrl(url: String) {
        API.request(.GET, url) { result in
            onComplete?(result: result)
        }
    }
}
```

#### Design View Controller

```swift
class MyViewController: UIViewController {

    let networkService = NetworkService()

    override func viewDidLoad() {
        super.viewDidLoad()
        networkService.onComplete = { result in
            print("I got \(result) from the server!")
        }
    }

}
```

```swift
@IBAction func didPressButton(sender: UIButton) {
  networkService.fetchDataFromUrl("http://localhost:4000/user")
}
```

Interestingly, when you use the closure way, `MYViewController` owns `NetworkService` and you are calling the method of `onComplete` within `MYViewController` instead of creating a couple floating functions within the view controller class. Often times, thsoe delegate functions are organized in a way that you can't follow.


### Comparison
We've so far looked into the both ways to deal with async tasks. Now, let us compare the pros and cons. First of all, the closure way certainly shorter, and `MYViewController` does not have to implement the async function. The `networkService` can call the method but `MYViewController` can simply ignore. Therefore, the closure way is often called, "decoupled". When you use the delegate way, you have to implement the method and `MYViewController` and `NetworkService` tightly go together.

Some would argue that you can make the delegate function as "optional". But, now you see your code look as below.

```swift
@objc protocol MyProtocol {
    @objc optional func doSomething()
}
```

> Aint' good.

When it comes to memory management, you have to define the protocol as `class` and define the delegate property as `weak` to prevent retain cycle. The closure way, when you work with `self`, you have to define it either using `unowned` or `weak`. If you are not using `self`, you don't have to worry about retain cycle.

> Again, if you are not comfortable with memory management or those terms above, feel free to enroll the course above.

If you want to communicate back to the `NetworkService`, using the delegate way, you have to use the datasource pattern by returning shown below.

```swift
func didCompleteRequest(result: String) -> String {
  print("Thanks for the data")
}
```

If you want to communicate back to the `networkService` object using the closure way, you also have to return something within `onComplete`.

```swift
var onComplete: ((result: String)->(String))? // return `String`
```

```swift
networkService.onComplete = { result in
    return "Thank you for helping, network object!"
}
```

If you want to work with many delegate, you probably have to do something like this,

```swift
extension MYViewController: UIViewController, GoogleMapDelegate, GoogleMapDataSource, UITableView, ... {

  let gooleMap = GoogleMap()
  let tableView = UITableView()
  let anotherObject = UIAnotherClass()

  override func viewDidLoad() {
      super.viewDidLoad()
      gooleMap.delegate = self
      tableView.delegate = self
      anotherObject.delegate = self
      }

   }

   func cellForRowAt indexPath: IndexPath) -> UITableViewCell {}
   func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {}

   // ...
   // ...
   // loads of other delegate functions
}
```

The code above looks hideous and you will have a hard time tracking and debugging. Good luck. Yes, we've tried to manage and distribute through `extension` but it will still be tough my friend.

But when it comes to the closure way, the only thing you need to do is


```swift
extension MYViewController {

  let gooleMap = GoogleMaps()
  let tableView = UITableView()
  let anotherObject = UIAnotherClass()

  override func viewDidLoad() {
    super.viewDidLoad()

    googleMap.getCurrentLocation = { location in
      print("Yay, I'm \(location")
    }

  }

}
```

But you notice the `getCurrentLocation` is an optional function that you don't need to handle. `MYViewController` can simply ignore. The example above is contrived and I've made up but I hope you get the point.

> Google Maps API uses `GMSMapViewDelegate` to handle async code.

### So far
Based on the analysis above, it seems like the closure is definitely winning in this case. Yes, that's the reason why many other platforms begin to shift from the delegate way to the closure way. Let's take a look at a couple other platforms.

### What do other platforms prefer?
Let's take a look at React Native using javascript.

```html
<View>
  <ListView
  dataSource={this.state.dataSource}
  renderRow={(rowData) => <Text>{rowData.title}, {rowData.releaseYear}</Text>}
  />
</View>    
```

You don't have to understand the statement above, but `=>` is a function in javascript. `rowData` is the parameter and what comes after is the rest of the closure block. `React` and `React Native` use the closure way.

Let's take a look at `RxSwift` which I love

```swift
buttton.rx.tap
     .debounce(1.0, scheduler: MainScheduler.instance)
     .subscribe(){ event in print("The user pressed the button!") }
     .addDisposableTo(disposeBag)
```

Again, you don't have to understand the code above. You will notice that after the `.subscribe` function, there is a callback/completion handler block.


Let's take a look at pure javascript
```javascript
function getMoviesFromApiAsync() {
  return fetch('https://facebook.github.io/react-native/movies.json')
    .then((response) => response.json())
    .then((responseJson) => {
      return responseJson.movies;
    })
    .catch((error) => {
      console.error(error);
    });
}
```

Again, you don't have to understand the code above, but it's all about callbacks and closures. In fact, javascript people love callbacks. Their lives are revolved around handling closures.


### What does Swift use?
As I've mentioned eariler, Swift API usese both the closure and the delegate way. However, there is a slight shift these days. One of examples is `UIAlerView`. In the past, there was a delegate protocol called,`UIAlertViewDelegate` which handles when the user has opened or closed the `AlertView`. Now it has been depreciated since [iOS 8](https://developer.apple.com/documentation/uikit/uialertviewdelegate).

For example, in the past `func alertViewCancel(UIAlertView)` and `func willPresent(UIAlertView)` used to exist to handle the action.

These days, the `AlertView` api utilities the closure way to notify us developers what kind of action the user has done with the `AlertView` object.

```swift
// Initialize Actions
  let yesAction = UIAlertAction(title: "Yes", style: .Default) { (action) -> Void in
      print("The user is okay.")
  }

  let noAction = UIAlertAction(title: "No", style: .Default) { (action) -> Void in
      print("The user is not okay.")
  }
```

### Conclusion
Yes, the world is shifting towards the closure way. However, the delegate pattern can't simply not exist because `UIViewController` is the delegate of the operating system, just fact that there is no `protocol`.  Built-in function such as `viewDidLoad`, `viewDidAppeear` are managed by the `UIApplication` singleton.

> Feel free to watch one of tutorials on Life Cycle of an iOS App on [YouTube](https://www.youtube.com/watch?v=mD8hsQjR1zk).

The delegate pattern is something we can't escape if you want an object to manage the function, not a human. It applies to front-end development, and android development. You can use either the delegate pattern or closure pattern to handle async tasks. But, you now understand why it is shifting towards the closure way.

### More
Other libraries exist such as `RxSwift`, `then`, `Async` exist to handle asynchronous code. They provide a way to handle async code. One thing to note is that they all use closures and completion handler approach over the delegate pattern.

### Thought on RxSwift
I might consider making a course on RxSwift and posts of them. If you are interested, feel free to get updated [here](https://bob413.typeform.com/to/ScDLy8).
