# What's new in Xcode 15

## Overview

- Watch and tvOs simulators optional
- All simulators are downloadable, Xcode will be smaller

## Editing
### Code Completion
- Xcode suggest the name of the struct to be the same name of the file
- Xcode editor suggests the most used modifiers for a certain View
- Color and image assets are now backed by Swift symbols, which means that they can now be code completed.

### Strings Catalogs
Strings catalogs pulls together your localizations into a single place so you can review and update your strings

Check the [String Catalogs](https://developer.apple.com/videos/play/wwdc2023/10155) section to learn more

### Documentation
New styling and spacing
New Documentaion preview

[Check more on Create rich documentation with Swift-DocC](https://developer.apple.com/videos/play/wwdc2023/10244)


#### Swift Macros
- [Expand on Swift macros](https://developer.apple.com/videos/play/wwdc2023/10167)
- [Write Swift macros](https://developer.apple.com/videos/play/wwdc2023/10166)

#### Preview API
#Preview {
    AppDetailColumn(screen: .account)
        .backyardBirdsDataContainer()
}

### Navigating
#### Bookmarks
- Anotates the line code, make it easy to see and remember what do 
- Bookmarks are a great reminders in my code
- You can sort and group your bookmarks together

### Sharing
Source control navigator

Uncommited changes

Now we are able to see both staged and unstaged changes, and change what is on the stage area thorugh Xcode

### Testing
Xcode is now 45% faster 

Insights
You can now see all the tests that took the longest time to run

[Fix failure faster with Xcode test reports](https://developer.apple.com/videos/play/wwdc2023/10175)


### Debugging
OSLog integration 
Full suport for OSLog
OSLog shows the severity
search for metadata and filter for metadata 

[See more on Debug with structured logging](https://developer.apple.com/videos/play/wwdc2023/10165/)

### Distributing
Distribute your app with XcodeCloud
Notarization for macOS
Xcode now offers signature verification for XCFrameworks

SDK owners can now create a PrivacyInfo manifest

[Verify app dependencies with digital signatures](https://developer.apple.com/videos/play/wwdc2023/10061)
[Get started with privacy manifests](https://developer.apple.com/videos/play/wwdc2023/10060)

- Testflight internal builds

[Simplify distribution with Xcode and Xcode Cloud](https://developer.apple.com/videos/play/wwdc2023/10224)

## References
- [What’s new in Xcode 15](https://developer.apple.com/videos/play/wwdc2023/10165/)

