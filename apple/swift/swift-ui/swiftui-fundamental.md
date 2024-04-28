# SwiftUI Fundamentals
In this note it'll concentrate all about SwiftUI's protocols and properties. It'll serve as a reference to see how SwiftUI works internally

## App protocol
An app that uses the SwiftUI app life cycle has a structure that conforms to the App protocol. The structure’s body property returns one or more scenes, which in turn provide content for display. The @main attribute identifies the app’s entry point.

## SwiftUI view file
SwiftUI view files declare two structures. The first structure conforms to the View protocol and describes the view’s content and layout. The second structure declares a preview for that view

```swift
struct ContentView: View {
    var body: some View {
        Text("Hello, World!")
    }
}

#Preview {
    ContentView()
}
```

