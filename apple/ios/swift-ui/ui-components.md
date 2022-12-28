# UI Components | SwiftUI
## VStack
The default spacing is 8 pt
Per default, the vstack centralized their children items, even if their parent view align them in a different position, such as leading or trailing.
See the example below: 


```swift
 ZStack(alignment: .leading) {
    Color.brown.opacity(0.5)

    VStack(alignment: .leading, spacing: 16) {
       Text("Hello")
       Text("My friend")
    }
}
```


## TextField


## References
- [Background Color with SwiftUI](https://levelup.gitconnected.com/background-color-with-swiftui-415fc661b31f)
