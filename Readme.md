# Development standards

Here, you can find detailed explanations on how we do code reviews, which architecture we use while developing, how to do proper unit testing, which libraries we use and for what. It also contains the coding standards section. You're free to suggest/modify it if you think it's necessary. You can do so by branching and creating a pull request when you're finished.

In this document:

* [General Notes](#general-notes)

* [Code Reviews](#code-reviews)

* [Coding Conventions](#coding-conventions)

* [External Libraries](#external-libraries)

* [Architecture](#architecture)

* [Unit Testing Guidelines](#unit-testing-guidelines)

- - - 

# General Notes

1.  All new projects are to be coded in Swift. Objective-C is still allowed when working on an older project with Objective-C code base. Consult with the project's dev lead or with your team to see if it makes sense to bridge Swift into it.

2.  All new projects should treat warnings as errors.

# Code Reviews

### Who should do the reviews?

Code reviews should be done by people/roles in the following order:

1.  Yourself

    Prior to actually creating a pull request you should go through all of the changes yourself. It's much easier to spot possible bugs and loose ends when you have all your changes laid out in one place. This should reduce other reviewers' work and also enable you to produce better pre-review code.

2.  Default reviewers

    Each project should have two default reviewers assigned, meaning people with subscription to project's pull requests.

    First reviewer should do code reviews by default while the second one should jump in if the first reviewer is unavailable for some reason.

3.  Explicitly added reviewers

    If your code contains a logic regarding a specific field, which one of you team mates is an expert in, it would be advisable to ask him/her to take a look at your code.

    If you're the project's dev lead, you'd also be advised to ask one of your team members to do a code review.

4.  All other team members

    Code reviews are a great learning tool and, here at Five, we use it as such. More often than not, younger team members just 'accept' their older peers' pull requests with little or no questions asked. This should not be the case. Instead, if you see a construct being used that you're not familiar with you're encouraged to post a question such as 'why is this being used here?' or 'what do we gain from using A as opposite to using B' etc.

### What should we pay attention to in a review?

1.  Architecture.

    Code should be properly structured into modules hierarchy. Not everything concerning a single functionality should be in one module. E.g. a view presenting a list of items should have separate UI logic, module abstraction, business logic etc.

    Protocols should be used where appropriate, e.g. when defining an interface, specific behavior, etc.

2.  Coding errors.

    Multi-threading is one of common problems people tend to disregard. Not everything can be solved just by delegating a callback/method/function to be executed on a background thread.  Access to all shared resources should be synchronized, and critical sections should be locked carefully to avoid potential deadlocks. Accessing object properties from a certain thread while the object has been released on another could lead to exceptions.

    Memory leaks are another common problem. Capturing a strong delegate reference, setting up timers which execute instance methods after a delay and never being invalidated will cause a retain cycle.

    Expensive-to-create objects such as NSDateFormatter should be created only once where possible, reducing the CPU and memory pressure. Once created, their format/parameters might be tweaked multiple times and adjusted for a certain use when needed.

    Reusable objects such as cells should always be reset prior to being used again. The 'prepareForReuse' method should contain blank state initialization and all possible background tasks should be canceled e.g. image downloads.

    Protocol conformance in header files [ObjC only] which could easily be private along with all the properties which should be in module file only.

3.  Logical flaws.

    These are pretty hard to catch if you're reviewing a code of a project you're not familiar with. As such, this kind of flaws are most likely to be caught by project's dev lead.

4.  Coding standards deviations.

    This does not mean that we should keep track/report every whitespace which is out-of-place, but basic coding standards agreed upon should be respected by each team member.

5.  Review fixes.

    It's up to the pull request's author to notify the reviewers that the pull request has been updated. After that, the default watchers should make sure all of the defects are actually fixed. Only then should the pull request be accepted.

### Procedure

*Note:* If you're about to add a large library/framework as a part of pull request's functionality, please add it directly to develop branch. This presents us with a cleaner pull request since there is nothing to review in added library files. This will also ensure that the library does not get taken into consideration when generating the diff. If, for any reason, this is not possible, ensure that you actually delete all library and/or binary files in a single commit just before you make the pull request. That way, only your source changes/additions can be reviewed. Upon finalizing the pull request, simply revert the commit with all of the deleted POD files, binaries etc. and test that everything is working as expected. If the answer's yes, you're free to merge.

1.  A pull request gets created.
2.  Specific reviewers are assigned. [optional]

    This step should, generally, be used only if a specific part of the code requires certain person's expertise.

3.  Review comments get acted upon and the code gets fixed.
4.  Default watchers approve the pull request if changes to be made are minor. [situational]

    If you're certain the person being reviewed won't forget to amend some of the things mentioned in the comments, and that the person's responsible enough not to introduce new irregularities, it's OK to approve the pull request in advance.

5.  Reviewers are notified that the request has been updated, and additional code review gets performed.
6.  The pull request gets approved by default watchers or, if there's further fixing to be done, code undergoes further changes [3].
7.  Pull request gets merged by the author.

### Which tools do we use?

We use Bitbucket for our code review procedure. It offers a nice GUI where we can describe the pull request, select source/destination branches and reviewers. We can also update the pull request later.

### What do we use reviews for?

1.  Unifying our code base.

    This part should be pretty straight forward. It makes it easy to upgrade and maintain your own code. If all developers code in the same way, than all of the code is your own code just as your own code is everybody else's.

2.  Creating more robust and high quality code.

    Tightly coupled classes are not good. Repetition is not good either. We aim to have loosely coupled, non-specific implementations where ever possible. Even more, components which could potentially be re-used on other projects should be made as generic as possible. This does not mean that a news application should be made generic enough so it could easily be turned into a 3D game. It just means that our code should be easily transfered to another app or extracted into a stand-alone component. This allows us to spend less time implementing a very similar feature on a new app to the one we've had in the previous. It also allows us to spend more quality time on other app aspects.

3.  Fixing repetitive mistakes.

    Each person should keep a checklist of his/hers most common/repetitive mistakes. You should not compile the list by yourself. Instead, let the reviewers point you at the right direction. Over time, this should allow you to get rid of such mistakes completely.

4.  Learning.

    Every code review acts as a two-way street. People being reviewed grow and improve their code while the reviewers are introduced to some nice new patterns, tricks and fresh insights. Being the last item on the list of code review values doesn't make it the least valuable. On the contrary, all of the previous values are nice side effects of learning through code reviews.

*Note:* Never do code reviews for over 30 minutes in a single session. If necessary, ask a team mate to take over a review (if it's urgent) or just do it later in the day.

# Coding Conventions

## General

1.  All singleton methods should contain 'shared' in their name. E.g. if a class' name is 'NotificationHandler' and it has a method which returns its singleton instance, a correct way of naming it should be 'sharedHandler' or 'sharedNotificationHandler'.

*Note:* Please note that, in general, the use of singletons is discouraged. Use dependency injection or some other mechanism whenever possible. E.g. we tend to create a singleton reference in navigation service and pass it along as a dependency to view models which need it.

2.  Do not call an ever-alive object a [SomethingManager](http://blog.codinghorror.com/i-shall-call-it-somethingmanager/).

3.  Use GCD for performing background async tasks. If you need cancelable background jobs use NSOperation.

4.  Long running tasks are to be performed on a background thread with their completion announced on the main thread.

5.  IBOutlets should be private.

## Swift
1.  Follow naming conventions from [The Official raywenderlich.com Swift Style Guide](https://github.com/raywenderlich/swift-style-guide).

2.  Constants are named using lowerCammelCase. E.g.
```
let lineYOffset = 2
```

3.  Use ‘// MARK: -’  to split implementation into multiple parts.

4.  We have a preference towards [closures](http://fuckingswiftblocksyntax.com) as opposed to selectors.

5.  Errors should be signaled by throwing exceptions.

## Objective-C

1.  Follow naming conventions from [Apple's documentation](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.htm).

2.  Category names should be composed in a way that they have a class name they extend + category name. E.g.
```
NSString+ParsingAdditions.h

@interface NSString (NSStringParsingAdditions)
```

3.  Constants are named using CammelCase. E.g.
```
static NSUInteger const NumberOfCurvePoints = 80;
```

4.  Property getters should not have 'get' in their name. E.g.
```
@property (nonatomic, getter=getFailureCount) NSUInteger failureCount; // This is wrong!
@property (nonatomic) NSUInteger failureCount; // This is right.
```

5.  Boolean property getters should contain 'is, wants, does, should' or some similar prefix as to present the property name in a form of a question.
```
@property (nonatomic, getter=getStandardCompliant) BOOL standardCompliant; // This is wrong!
@property (nonatomic, getter=isStandardCompliant) BOOL standardCompliant; // This is right.
```

6.  Use [#pragma mark -](http://nshipster.com/pragma/) to split implementation into multiple parts.

7.  We have a preference towards [blocks](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml?showone=Blocks#Blocks) as opposed to selectors.

8.  When dealing with 'NSString' properties make sure you always [copy](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml?showone=Setters_copy_NSStrings#Setters_copy_NSStrings) them.

9.  Do not use [@synthesize](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml?showone=Automatically_Synthesized_Instance_Variables#Automatically_Synthesized_Instance_Variables).

10.  Prefer properties to instance variables (ivars).

11.  A property should always be explicitly defined either as 'atomic' or 'nonatomic'.

12.  Conform to Objective-C [literal syntax](http://www.thinkandbuild.it/guided-tour-through-objective-c-literals/).

13.  Use Objective-C types instead of plain C types. E.g. use 'NSInteger' instead of and 'int', CGFloat instead of 'float' etc.

14.  Errors should be passed around as NSError pointers.

# [External Libraries](Libraries/Libraries.md)

# Architecture

To produce more stable and resilient apps, to enable quantifiable measure of code quality and to make developers understand each other's code easier, the architecture used in the app development has to be defined and standardized.

The Model-View-ViewModel architecture, based on the experience in the team as well as on the experience of iOS community worldwide, proves to be optimal answer for requirements of iOS app development.

Adhering to MVVM architecture enables apps to be built with clear separation of concerns, especially separation of View and Model concerns.

Apps based on MVVM architecture are comprised of following components:

* Navigation Service
* View
* ViewModel
* Model
    * Entity
    * Store

## Model

Model, often greatly neglected, represents foundation of each app. It provides app with the data and domain-specific knowledge. All business logic should be contained withing the Model. Model respresents the client service - both the data the service provides and the operations that can be performed on that data.

It's comprised of Stores and Entities. Store represents a resource storage or repository. An API instance, a CoreData stack or a Realm would be examples of a Store. Store provides methods for fetching and managing the Entities. Entities represent actual resources (model objects).

Model handles caching and persistance. Upper components of the app (ViewModel and View) must have no knowledge of that. Model can internally use more than one Store to handle it, but only one compound Store will be exposed to the upper components of the app in that case.

Upper components interact with one or more Stores to fetch, update or delete Entities. Neither a Store nor an Entity should have knowledge of any upper component.

Model should be build in a way that it can potentially be used as a foundation of other apps. For example, a phone and a watch app should differ in upper components (View and ViewModel), but should use same Model component because that's the component that encapsulates whole business logic.

## ViewModel

ViewModel is a central component in MVVM architecture and has two tasks. It must provide and expose the data that the View displays and it must be able to receives events from the View and act upon them, usually by updating the Model.

ViewModel maps the data from the Model to a user-friendly format ready for the View to display. It knows what actions trigger what operations on the Model Stores. ViewModel is a layer that separates the Model domain from the View domain. Model and View must know nothing about each other.

ViewModel's interaction with the View should be kept at minimum. Communication from ViewModel to View should be limited only to the notifications that the exposed data changed - so that the View can update itself. ViewModel's interaction with the View should be eliminated by exposing data using an observable/bindable types (KVO/Reactive).

## Navigation Service

There will be one central component in charge of navigation - the Navigation Service. ViewModels that as a response to some user action need to present some other ViewController shall ask Navigation Service to present that ViewController.

Navigation service creates ViewModel and initializes view controller with it. This way, navigation service is in charge of dependency injection for ViewModel and can inject only those dependencies which ViewModel directly uses. 

*Note:* Navigation Service's transitions should all be tracked via remote logging e.g. Crashlytics custom logging so we can always see the exact (per user) retro steps in case of debugging crash logs.

## View

Main responsibility of the View is to define what user will see. The data that needs to be displayed is provided by the ViewModel. View has no knowledge of where the data is really coming from (APIs, persistent stores, etc). View only reads the data exposed in the ViewModel and displays it to the user.

Additionally, View is responsible for receiving user events like touches, scrolls, orientation changes, etc. Events that should cause side effects in the Model should be propagated to the ViewModel. Other events, like ones that impact rendering and layout (for example orientation change), should be handled by the View.

View controllers are part of the View. They should be treated same as any other view but with one addition - they can also handle navigation, either by themselves or by a separate routing component.

A View (or a ViewController) should define its ViewModel with a protocol. Mapping between the View and its ViewModel interface is always 1:1. More than one implementation can be provided for View's ViewModel interface.

## Implementation guidelines

Model Entities should always be exposed through protocols. Upper components of the app should work only with those protocols and not with JSON, NSManagedObject or similar loosely typed objects.

```swift
class Monkey: NSManagedObject {}

protocol MonkeyType: AnyObject {
  var id: Int { get }
  var name: String { get }
}

extension Monkey: MonkeyType {}

class Banana: NSManagedObject {}

protocol BananaType: AnyObject {
  var id: Int { get }
  var name: String { get }
  var owner: MonkeyType { get }
}

extension Banana: BananaType {}
```

Stores should always provide strongly typed interface based on Entities.

```swift
protocol API {
  func fetchBananasForMonkey(monkey: MonkeyType, completion: [BananaType] -> Void)
  func createBananaForMonkey(monkey: MonkeyType, bananaName: String, completion: BananaType -> Void)
  func eatBanana(banana: BananaType, completion: () -> Void)
}
```

Stores that internally represent objects in different types must do conversions to immutable structs. Entities can be extended with conversion methods.

```swift
extension Monkey: JSONEncodable {
  init(json: AnyObject) throws
}

extension Monkey: JSONDecodable {
  func toJSON() -> AnyObject
}
```

Components that work with Model and its Stores, like View and ViewModel components, must know nothing about JSON, NSManagedObject, RealmObject, API parameters, HTTP methods, etc. That should all be contained within the Model itself.

Each View (or a ViewController) should define its ViewModel with a protocol. It's recommended to suffix protocols with _Type_.

```swift
protocol BananaViewModelType {
  var myNameText: String { get }
  var myBananasText: String { get }
  func eatSomeBanana()
}
```

View then works only with the objects conforming to that ViewModel protocol. View holds a strong reference to the ViewModel.

```swift
class BananaView {
  let myNameLabel: UILabel
  let myBananasLabel: UILabel

  var viewModel: BananaViewModelType! {
    didSet {
      myNameLabel.text = viewModel.myNameText
      myBananasLabel.text = viewModel.myBananasText
    }
  }

  @IBAction func didTapEatBananaButton() {
    viewModel.eatSomeBanana()
  }
}
```

Concrete ViewModels for that View should conform to its ViewModel protocol. ViewModel usually holds a strong reference to the Store it works with. Store should be injected into the ViewModel (i.e. passed in the constructor). ViewModel should not make use of shared (singleton) objects.

```swift
class BananaViewModel: BananaViewModelType {
  var myNameText: String = ""
  var myBananasText: String = ""

  let me: Monkey
  let api: API

  var myBananas: [Banana] = [] {
    didSet {
      myBananasText = myBananas.map { $0.name }.joinWithSeparator(", ")
    }
  }

  init(me: Monkey, api: API) {
    self.me = me
    self.api = api
    self.myNameText = me.name
  }

  func eatSomeBanana() {
    if let banana = myBananas.last {
      api.eatBanana(banana) {
        self.myBananas.removeLast()
      }
    }
  }
}
```

# Unit Testing Guidelines

In order to insure our apps work as expected we will cover business logic with unit tests. MVVM architecture components that ought to be tested are ViewModel, Store and Model.


## Message types by origin

1. Incoming
  -  Messages sent to object under test by other objects.
2. Outgoing
  -  Messages object under test sends to other objects.
3. Private
  - Messages object under test sends to itself.

## Method types

1. Query
  - Return something, change nothing.
  - No side effects.
2. Command
  - Don't return anything.
  - Observable side effects.

## Unit testing principles

1. Test incoming query messages by inspecting the result they return
   
   Example: UITableDatasource - *numberOfRowsInSection:*, *cellForRowAtIndexPath:*

2. Test incoming query messages by asserting public side effects that they cause
    
    Example: *viewDidLoad:* method.

3. Do not test private messages
   -  They should be covered with tests for other cases.

4. Do not test outgoing query messages
   - No need to test them since they are incoming query messages for some other object.
   - Use stubs when testing objects that send this type of messages.

5. Expect outgoing command message to happen
  - Use mocks for asserting.


Every unit test should have following structure:

```
func testClamp() {
    // Given
    let floatValue = Float(-1.0)

    // When
    let clampedFloatValue: Float = Math.clamp(floatValue, between: 0.0, and: 1.0)

    // Then
    XCTAssertEqual(clampedFloatValue, 0, "Clamped value should always be " +
                                         "higher or equal to the lower bound.")
}
```
