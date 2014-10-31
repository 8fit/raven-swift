# raven-swift

Swift client for [Sentry](https://www.getsentry.com/welcome/).


## Installation

TODO:
~~The easiest way is to use [CocoaPods](http://cocoapods.org). It takes care of all required frameworks and third party dependencies:~~

```ruby
pod 'raven-swift'
```

**Alternatively**, you can install manually.

1. Get the code: `git clone git://github.com/getsentry/raven-swift`
2. Drag the `RavenClient.swift` and `RavenConfig.swift` files to your project. Check both "copy items into destination group's folder" and your target.
3. If you want to set up a global exception handler, drag the `UncaughtExceptionHandler.h` and `.m` files to your project. Check both "copy items into destination group's folder" and your target.

Alternatively you can add this code as a Git submodule:

1. `cd [your project root]`
2. `git submodule add git://github.com/getsentry/raven-swift`
3. Drag the `RavenClient.swift` and `RavenConfig.swift` files to your project. Uncheck the "copy items into destination group's folder" box, do check your target.
4. If you want to set up a global exception handler, drag the `UncaughtExceptionHandler.h` and `.m` files to your project. Check both "copy items into destination group's folder" and your target.


## How to get started

While you are free to initialize as many instances of `RavenClient` as is appropriate for your application, there is a shared singleton instance that is globally available. This singleton instance is often configured in your app delegate's `application:didFinishLaunchingWithOptions:` method:

```swift
 func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {
        
        RavenClient.clientWithDSN("https://30d629f2df9c4fdf8507e1704c09a526:f766cf8e0fff446986ac6daf1902e832@app.getsentry.com/888")

        // [...]
        return true
}
```
The first `RavenClient` that is initialized is automatically configured as the singleton instance and becomes available via the `sharedClient` singleton method:

```swift
println("I am your RavenClient singleton : \(RavenClient.sharedClient?)")
```

```swift
// Sending a basic message (note, does not include a stacktrace):
RavenClient.sharedClient?.captureMessage("TEST 1 2 3")

// Sending a message with another level and a stacktrace:
RavenClient.sharedClient?.captureMessage("TEST 1 2 3", level: .kRavenLogLevelDebugInfo, method: __FUNCTION__, file: __FILE__, line: __LINE__)
```

You can also capture errors:

```swift
var error: NSError?
NSFileManager.defaultManager().removeItemAtPath("some/path", error: &error)

// Sending basic error 
RavenClient.sharedClient?.captureError(error!)

// Sending error with method, file and line number 
RavenClient.sharedClient?.captureError(error!, method: __FUNCTION__, file: __FILE__, line: __LINE__)
```

### Handling exceptions

If you want a global exception handler, you will need to add this to your [bridging header](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/BuildingCocoaApps/MixandMatch.html): 
```objective-c
#import "UncaughtExceptionHandler.h"
```
and in `UncaughtExceptionHandler.m` replace the 
```objective-c
#import "Raven-Swift.h"
```
with
```objective-c
#import "YourProductModuleName-Swift.h"
```

Then you can set up a global exception handler:

```swift
func application(application: UIApplication, didFinishLaunchingWithOptions launchOptions: [NSObject: AnyObject]?) -> Bool {
    // Override point for customization after application launch.
    
    RavenClient.clientWithDSN("https://[public]:[secret]@[server]/[project id]")
    
    RavenClient.sharedClient?.setupExceptionHandler()

    return true
}
```

*Note: when using the global exception handler, exceptions will be sent the __next__ time the app is started.*


## Issues and questions

Have a bug? Please create an issue on GitHub!

https://github.com/getsentry/raven-swift/issues


## Contributing

raven-swift is an open source project and your contribution is very much appreciated.

1. Check for [open issues](https://github.com/getsentry/raven-swift/issues) or [open a fresh issue](https://github.com/getsentry/raven-swift/issues/new) to start a discussion around a feature idea or a bug.
2. Fork the [repository on Github](https://github.com/getsentry/raven-swift) and make your changes.
3. Make sure to add yourself to AUTHORS and send a pull request.


## License

raven-swift is available under the MIT license. See the LICENSE file for more info.
