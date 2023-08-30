# Track down hangs with Xcode and on-device detection
## What are hangs
A hang is reported when the main thread is busy doing work
to the user it appears that the app is completely stucked

### Negative user experience
People avoid appas that are frequently unresponsive
- Force quits
- Switch apps
- Delete your app

## Development tooling
- Thread perfomance checker
- Hand detection in Instruments

main thread has no cpu usage -> that means that it's waiting for another thread to complete its work
thread

hang tracing instrument
In addition to hang detection and labeling, to find specific periods of unresponsiveness


## Beta tooling
### On-Device Hang Detection
Go to Settings -> Developer -> Hang Detection -> Enable Hang Detection

If you run your app on your physical device, you should see it on the list of monitored apps

After enabling it, begin browsing around within your app to identify areas where the app experiences a hang. When the hang detection tool detects a hang, it will display a pop-up notification indicating the duration of the hang and will organize them by timestamp

## Public release tooling
Xcode Organizer collect hang reports

For each signature, you can find a few sample hang logs.
Each hang log contains the main thread stack trace containing the code responsible for th ehang, the hang duration, and the device and OS version from which the log originated

Each sinature also provides aggregate statistics about how many hang logs the singature was responsible for and a breakdown of those logs per each OS version and the device type

## Submitting symbols
We should submit our apps to the AppStore with symbol information
Improves Xcode Organizer experience
Only essential information extracted
Securely stored and never shared

## Summary
- Fix hangs early
- Enable Thread Perfomance Checker
- Enable on-device hang detection
- Use hang report regularly
- Enable regression notifications

## Resources
- [Understand and eliminate hangs from your app](https://developer.apple.com/videos/play/wwdc2022/10082)
- [Diagnosing performance issues early](https://developer.apple.com/documentation/xcode/diagnosing-performance-issues-early)
- [Identify trends with the Power and Perfomance API](https://developer.apple.com/videos/play/wwdc2022/10057)
- [Diagnose Power and Perfomance regressions on your app](https://developer.apple.com/videos/play/wwdc2021/10087)


