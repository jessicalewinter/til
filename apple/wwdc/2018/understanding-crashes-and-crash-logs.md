# Understanding Crashes and Crash Logs

## What is a crash? 
A crash is a sudden termination of your app when it attempts to do something that is not allowed. Below you have a list of things that may result on a crash:

- Impossible for the CPU to execute code
- Operating system is enforcing a policy
- Programming language is preventing a failure, i.e, index out-of-bounds
- Developer is preventing a failure, i.e, custom fatalError and assertionFaiulures


The OS will guarantee an UX by killing the app if it's taking too long to launch or it's using too much memory.If it's taking too much to launch or memory issue

Attached to the debugger, save to the disk on a human redadable format
A symbolicated crash log we can see the function, variables and file names on it
Unique devices affecteed

App Start -> Crash point -> App termination

Unsymbolicated crash logs bunch of binaries location. 

## Fundamentals
### Crashes Organizer
Open Xcode Organizer to check all your app crashes

- Testflight and AppStore apps
- All platforms, app extensions
- Recent Stattistics
- Most affected devices
- Page through logs
- Open log in debugger
- Crash location highlighting

#### Getting Started
- AppStore users opt in
- Sign in to Xcode
- Upload app with symbols
- Open crashes tab of the Organizer window

### Acessing Crash Logs
- Organizer Window
- Devices Window
- Automated testing
- Console app
- Sharing from device

When you upload your app, we should include symbols so that you get server-side symbolication of your crash logs
Under Settings -> Privacy -> Analytics -> Analytics Data

#### Symbolication
Best Practives
- Upload app symbols for server-side symbolication
- Keep your app archive for local symbolication
- Download debug symbols for bitcode apps

## Accessing crash logs
Summary -> OS information
Crash reason -> Crashed Thread
Crash message -> Application Specific Information(Not always available)
Stack trace -> backtrace of all threads that was running at the time of the crash. The log will highlight which thread was responsible for the crash.
Register state->
Binary Images -> Application executable and all other libaries. Xcode uses this data for symbolication purposes, so it can look up the symbols, the files and line number for the stack traces.

All of this all together is part of a crash log.
We start taking a look at crash reason, the Exception type field.
We can have an EXC_BAD_INSTRUCTION(SIGILL). That means the CPU is trying to execute an instruction that does not exist or is invalid for some reason, which terminates the app. Besides that, we can look at the code, thread crashed and the code that was being executed.

The Application Specifc information includes the error message that the Swift runtime throws the reason of the failure.

### Assertions and Preconditions
Deliberately halt the process when an error has occurred.
Ex:
- Forced unwrap of an `Optional` that store nil
- Out-of-boundds `Swift.Array`access
- Swift arithmetic overflow
- Uncaught exception
- Custom assertion

### Terminations by the Operation System
- Watchdog events, i.e, timeout. If your app takes too long to execute something, the OS can detact that, kill the process that generates a particular ctash log
- Device overheated. Environment conditions can be responsible for killing the app. If a device is overhating the OS will kill all the processes that are using too much CPU. 
- Memory exhaustion -> Also, if the device is running out of memory, it will kill processes that is consuming a lof of memory.


## Analyzing crash logs


## Memory errors


## Multithreading issues


## References
- [Understanding Crashes and Crash Logs](https://developer.apple.com/videos/play/wwdc2018/414/)
