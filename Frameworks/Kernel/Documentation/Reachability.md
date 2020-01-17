# Reachability.swift

<!---
[![CI Status](http://img.shields.io/circleci/crushroom/Keychain.svg)](https://circleci.com/crushroom/Keychain)
-->

[![Platform](https://img.shields.io/static/v1?label=platform&message=ios%20|%20macOS&color=lightgrey)]()


Reachability.swift is a replacement for Apple's Reachability sample, re-written in Swift with closures.

It is compatible with **iOS** (8.0 - 12.0), **OSX** (10.9 - 10.14) and **tvOS** (9.0 - 12.0)

Inspired by https://github.com/tonymillion/Reachability

## Got a problem?

Please read https://github.com/ashleymills/Reachability.swift/blob/master/CONTRIBUTING.md before raising an issue.

## Example - closures

NOTE: All closures are run on the **main queue**.

```swift
// Declare this property where it won't go out of scope relative to your listener
let reachability = Reachability()!

reachability.whenReachable = { reachability in
    if reachability.connection == .wifi {
        print("Reachable via WiFi")
    } else {
        print("Reachable via Cellular")
    }
}

reachability.whenUnreachable = { _ in
    print("Not reachable")
}

do {
    try reachability.startNotifier()
} catch {
    print("Unable to start notifier")
}
```

and for stopping notifications

```swift
reachability.stopNotifier()
```

## Example - notifications

NOTE: All notifications are delivered on the **main queue**.

```swift
// Declare this property where it won't go out of scope relative to your listener
let reachability = Reachability()!

// Declare this inside of viewWillAppear

NotificationCenter.default.addObserver(self, selector: #selector(reachabilityChanged(note:)), name: .reachabilityChanged, object: reachability)
do {
    try reachability.startNotifier()
} catch {
    print("could not start reachability notifier")
}
```

and

```swift
@objc func reachabilityChanged(note: Notification) {
    let reachability = note.object as! Reachability

    switch reachability.connection {
    case .wifi:
        print("Reachable via WiFi")
    case .cellular:
        print("Reachable via Cellular")
    case .unavailable:
        print("Network not reachable")
    }
}
```

and for stopping notifications

```swift
reachability.stopNotifier()
NotificationCenter.default.removeObserver(self, name: .reachabilityChanged, object: reachability)
```

## Want to help?

Got a bug fix, or a new feature? Create a pull request and go for it!

## Let me know!

If you use **Reachability.swift**, please let me know about your app and I'll put a link [here…](https://github.com/ashleymills/Reachability.swift/wiki/Apps-using-Reachability.swift) and tell your friends!

Cheers,
Ash
