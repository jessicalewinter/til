# Built-in methods

## `count`
To check whether a collection is empty, use its isEmpty property instead of comparing count to zero. Unless the collection guarantees random-access performance, calculating count can be an O(n) operation.

### Complexity
Array conforms to RandomAccessCollection, so the complexity O(1)
String does not conforms to RandomAccessCollection, so the complexity is O(n)

## `isEmpty`
When you need to check whether your collection is empty, use the isEmpty property instead of checking that the count property is equal to zero. For collections that don’t conform to RandomAccessCollection, accessing the count property iterates through the elements of the collection.

### Complexity
O(1)


