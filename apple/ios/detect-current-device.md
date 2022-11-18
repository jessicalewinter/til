# Detect current device

I have found that my layout was breaking when I returning a newgative node, I have found later that the best solution was to absolutize my viewDifference, see the solution below:

```swift
abs(viewHeight - groundNode.position.y)
```

At first, I thought that I needed to calculate a difference size for different devices, like iPhone and iPad, and that makes me search for and find this API UIUserInterfaceIdiom that I can lap through this enum and do different logic, tha directly deppend of the device you're running your app/
Curious, hum?

```swift
func currentDevice() -> String {
	let currDevice = UIDevice.current.userInterfaceIdiom
	
	switch currDevice {
        case .unspecified:
            return "Unspecified \(deviceString)"
        case .phone:
            return "iPhone \(deviceString)"
        case .pad:
            return "iPad \(deviceString)"
        case .tv:
            return "TV \(deviceString)"
        case .carPlay:
            return "CarPlay \(deviceString)"
        case .mac:
            return "Mac \(deviceString)"
        }
}
print(currentDevice)
```

## References
- [Detect current device with UIUserInterfaceIdiom in Swift](https://www.tutorialspoint.com/detect-current-device-with-uiuserinterfaceidiom-in-swift)
- [How to detect your device with swift 4(iPhone or iPad)](https://medium.com/daily-ios/how-to-detect-your-device-with-swift-4-iphone-or-ipad-244c365d0472)
