# Logging API #

<!--- [![CI Status](http://img.shields.io/circleci/crushroom/Logging.svg)](https://circleci.com/crushroom/Logging) -->

[![Platform](https://img.shields.io/static/v1?label=platform&message=ios%20|%20macos%20|%20tvos%20|%20watchos&color=lightgrey)]()

`Logging` API is a Swift centric wrapper for the iOS `os_log`'ing feature.

## :bulb: Features

- Simple interface
- Uses `os_log`
- **[Swift 5.0 compatible](#requirements)**
- `RELEASE` & `DEBUG` support.
- Builtin filtering

## :book: Usage

The design of the logging system is meant to be at the same level as `print` but with the control of `DEBUG` versus `RELEASE` builds as well as runtime control over the enabling of logging. Given that logging will go out through the `os_log()`  system, logging will also appear in the console application.

There are functions for each type of logging levels.

- `.debug()`
Log a message with the implication that it is for real time debugging only. A no-op in `RELEASE` builds. `Debug` implies that if you should remove when you are done debugging.

- `.info()`
Log a message with the implication that it is for information purposes. A no-op in `RELEASE` builds. `Info` implies that this is useful for any developers to leave in the code.

- `.warn()`
Log a message with the implication that it is callling out an unusual sitation. **Will be logged in both `DEBUG` as well as `RELEASE` builds.** `Warn` implies that this is used to show unusual situations. Should be used in conjunction with `DEBUG` `asserts`. The asserts will not fire in debug builds, but the `Warn` level logging will log.

	_Please be careful about using this as it will potentially leak implementation details as well as `PII`._

- `.error()`
Log a message with the implication that it is calling out a fatal error situation. **Will be logged in both `DEBUG` as well as `RELEASE` builds.** `Error` implies that this is used to critcal/fatal situations.

	> Please be careful about using this as it will potentially leak implementation details as well as `PII`.
	> Think about using the provisions around PRIVATE/PUBLIC in the os_log() system. See the section on *Privacy Level*

You can configure the `Logger` in real time to change the behavior of the logging system.  You can turn each of the levels on and off.

```swift
Logger.info("The http payload is: \(payload.data)")
...

Logger.disable(.info)	// Turn OFF all .info type of logging

...

Logger.enable(.info)	// Turn ON all .info type of logging
```

See also `Logger.disableAll()` and Logger.enableAll().

> NB: You cannot enable `.debug` and `.info` types in `RELEASE` builds, but you can disable all types.

Turning off the `Error` and `Warn` levels will have no effect in release builds, and given that the others don't produce anything in `RELEASE` builds, turning them on or off has no impact.

### Privacy Levels and *os_log()*

The `Logger` by default sends all `.info` & `.debug` logs as `%{PUBLIC}@` and all `.warn` and `.error` as `%{PUBLIC}@`.
This is in an effort to keep from logging PII.

```swift
Logger.info("The user \(userName) is logged in")
Logger.error("The user is \(userName) is logged in")
```

The Xcode console and the Console.app will show the data as normal when a debugger is attached.

```
LogExample[7784:105423] [crushroom] The user woolie logged in
LogExample[7784:105423] [crushroom] The user woolie logged in
```

However, opening the app while no debugger is attached will show the following output in the Console.app.

```
debug   18:58:40.532132 +0100   LogExample  The user woolie logged in
debug   18:58:40.532201 +0100   LogExample  The user <private> logged in
```
The username is logged as `<private>` instead which prevents your data from being readable by anyone inside the logs.

## Requirements

|            | OS                                     | Swift         |
|------------|----------------------------------------|---------------|
| **v1.0.x** | iOS 12.2+, OSX 10.14+, watchOS 5+, tvOS 13+      |Swift 5+|

# Attribution #

Logging was originally authored by `Steven Woolgar` for Crushroom.

## Author

Steven Woolgar, steven_woolgar@woolsoft.com

## License

Logging is owned by Crushroom. No license available.
