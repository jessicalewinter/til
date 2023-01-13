## FRP
This means that FRP enables us to write code that can be composed using many small functions that operate only on their inputs without 
changing anything that’s outside of the function itself.

An example is the map function
```swift
let inputArray = [1, 2, 3]
let outputArray = inputArray.map { $0 * 2}
print(outputArray)
```

The map function only operates on the array that it’s called on and instead of changing the existing array, it returns a brand-new, mapped array.


### High-Order Functions
It's a A function that takes another function or closure as its parameter

### Pure functions
It's a a function that only operates on the arguments it receives is.


## References
- [Practical Combine | Donny Wals](https://practicalcombine.com/)
