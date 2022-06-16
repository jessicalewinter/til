# Array fundamentals
## Random access

Random-access is a trait that data structures can claim if they can handle element retrieval in a constant amount of time. To retrive, for example, the second element of an array takes constant time.

## Insertion location
Using the *append* method to insert an element into an array takes constant time 

## Predetermined array capacity
Underneath the hood, Swift arrays are allocated with a predetermined amount of space for its elements.If you try to add new elements to an array that is already at maximum capacity, the Array must restructure itself to make more room for more elements
Each time it runs out of storage and needs to copy, it doubles the capacity.

