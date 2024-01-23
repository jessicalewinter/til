
## UIImage.prepareForDisplay
If you need to load a large image from your local storage, if you don't dispatch it to a background thread, your main thread will be blocked waiting for the result of this operation.

On iOS 15, it was introduced a new method to decode an image to an UIImage on a background threa, and then we can call a completion handler once the decoded image is ready to be displayed. You need to remember to switch back to the main thread so you can assign the imageView.image on the main thread to avoid a crash.

```swift
    let image = UIImage(named: "Big-Image")
    let imageView = UIImageView()
    image?.prepareForDisplay(completionHandler: { newImage in
        imageView.image = newImage
    })
```

