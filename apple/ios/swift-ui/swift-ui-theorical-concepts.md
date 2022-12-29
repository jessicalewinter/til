# SwiftUI Theorical Concepts

## ViewModifier


## PropertyWrappers

### @EnvironmentObject

### @Environment

### @Published

### @State

### @StateObject
The fact that nothing changes is precisely why I often decided to use @ObservedObject.
However, there’s a significant difference in making it clear when to use @StateObject instead of @ObservedObject.

Observed objects marked with the @StateObject property wrapper don’t get destroyed and re-instantiated at times their containing view struct redraws.
Understanding this difference is essential in cases another view contains your view.

It’s unsafe to create an @ObservedObject inside a view since SwiftUI might create or recreate a view at any time. Unless you inject the @ObservedObject as a dependency, you want to use the @StateObject wrapper to ensure consistent results after a view redraw.

### @ObservedObject
@StateObject and @ObservedObject have similar characteristics but differ in how SwiftUI manages their lifecycle. 
Use the state object property wrapper to ensure consistent results when the current view creates the observed object.i
 Whenever you **inject** an observed object as a dependency, you can use the @ObservedObject.

### @ViewBuilder


## References
