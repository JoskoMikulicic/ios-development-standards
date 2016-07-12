# External Libraries

External libraries should be added to projects via the following methods ordered by priority (if a method of higher priority is not available, use the next one e.g. if a project does not have PODs, revert to submodule).

1.  POD files.

2.  Git Submodules.

*Note:* POD sources should not be committed along with the project's source files.

Following is the list of external libraries used throughout the projects:

Each library should have the following defined:

1.  Repository URL.

2.  Short description.

3.  Category e.g. 'Imaging', 'Networking', 'Crash reporting', 'Caching' etc.

4.  Known/encountered bugs. These should not be a subset of github reported issues but issues we've encountered during libraries' usage.

*Note:* If not for Swift, the library should have an *[Objective-C]* mark by its name.

## Categories

* [Animation](Animation.md)
    * [iOSCurveInterpolation](Animation.md#iOSCurveInterpolation)
    * [MarqueeLabel](Animation.md#MarqueeLabel) [Objective-C]
    * [JDStatusBarNotification](Animation.md#JDStatusBarNotification)
* [Asynchronous Programming](AsynchronousProgramming.md)
    * [PromiseKit](AsynchronousProgramming.md#PromiseKit)
    * [AsyncSwift](AsynchronousProgramming.md#AsyncSwift)
* [Caching](Caching.md)
    * [TMCache](Caching.md#TMCache) [Objective-C]
    * [Haneke](Caching.md#Haneke)
* [Camera](Camera.md)
    * [FDTake](Camera.md#FDTake)
* [Imaging](Imaging.md)
    * [UIImage+Color](Imaging.md#UIImage+Color)
    * [UIImage+ImageEffects](Imaging.md#ImageEffects)
* [Layout](Layout.md)
    * [DTCoreText](Layout.md#DTCoreText) [Objective-C]
    * [PureLayout](Layout.md#PureLayout) [Objective-C, Swift]
    * [TZStackView](Layout.md#TZStackView)
* [Messaging](Messaging.md)
    * [Layer](Messaging.md#Layer) [Objective-C]
    * [Atlas](Messaging.md#Atlas) [Objective-C]
* [Networking](Networking.md)
    * [AFNetworking](Networking.md#AFNetworking) [Objective-C]
    * [Reachability](Networking.md#Reachability)
    * [OHHTTPStubs](Networking.md#OHHTTPStubs) [Objective-C, Swift]
    * [Alamofire](Networking.md#Alamofire)
    * [Moya](Networking.md#Moya)
* [Reactive Programming](ReactiveProgramming.md)
    * [ReactiveCocoa](ReactiveProgramming.md#ReactiveCocoa)
* [Sharing](Sharing.md)
    * [DMActivityInstagram](Sharing.md#DMActivityInstagram)
    * [JBWhatsAppActivity](Sharing.md#JBWhatsAppActivity)
* [UnitTesting](UnitTesting.md)
    * [OCMock](UnitTesting.md#OCMock)
    * [Expecta](UnitTesting.md#Expecta) [Objective-C]
    * [Specta](UnitTesting.md#Specta) [Objective-C]
    * [Nimble](UnitTesting.md#Nimble) [Objective-C, Swift]
* [UserManagment](UserManagment.md)
    * [LoginRadius](UserManagment.md#LoginRadius)
