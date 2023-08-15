# Sharing Resources
Each time you call subscribe(...), this creates a new Observable for that subscription — and each copy is not guaranteed to be the same as the previous. 
The Observable could produce the same sequence of elements, but it isn't necessary to produce those same duplicate elements for each subscription.
To share a subscription, you can use the share() operator. A common pattern in Rx code is to create several sequences from the same source Observable by filtering out different elements in each of the results.


`share` create a subscription only when the number of subscribers goes from 0 to 1. When new subscribers start observing the sequence, share uses the already created subscription to share with them. 
If all subscriptions to the shared sequence get disposed, share will dispose the shared sequence as well. 
If another subscriber starts observing, share will create a new subscription for it just like described above.

> Note: share() does not provide any of the subscriptions with values emitted before the subscription takes effect. `share(replay:scope:)` store the last few emitted values and can provide them to new observers upon subscription.



## Resources
- [RxSwift: share()-ing is Caring](https://medium.com/gett-engineering/rxswift-share-ing-is-caring-341557714a2d)


