# Swift Package Plugin
I was developing my Network Layer written in swift, and I wanted to localize some of API Errors that I handlded, and then I thought that I could introduce in my spm package the famous code generator Swiftgen, but then I have faced a problem:
Until swift 5.6, I have no way to add swiftgen to a spm package. I have to manually create my Strings enum from a Localizable file. But this have change at WWDC22, that introduced the newly Swift Package Build Plugin API. Now I'm able to create a custom plugin to adding custom steps in the build and pre-build phase.


## References
- [Implement Your First Swift Package Build Plugin](https://betterprogramming.pub/implement-your-first-swift-package-build-plugin-9835a7aded0b)

