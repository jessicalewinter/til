# Understand and eliminate hangs from your app
## What are hangs
A period of unresponsiveness is called a hang
> Hang aka Unresponsive aka Laggy aka Slow aka Spinning aka Stuck

### The main runloop
The main runloop is a loop your app's main thread enters to run event handlers in response to incoming events, primarily user interactions.
When a user interacts with an app, the runloop receives the event, process it and the updated the UI if needed. This process repeats for each user input.
If an event takes a long time to be processed, it'll be a delay to update the UI. Events are buffered while the main thread is busy and cannot be handled by it during a hang. It'll only handled after the current handle terminates

A delay of over 1 second will be considered a hang, but in some cases a hang of 0.5 seconds can cause a exttemous bad user experience, like scrolling your items. But if a hang of 0.5 seconds will be less noticed when a view is delaying to show items when is loaded.

By eliminating hangs you apps will look smooth as never.
> Snappy aka Smooth aka Quick aka Responsive aka Fast

## What causes hangs
A hang occurs when there's too much work being done on the main thread.
A hang on the main thread can be caused by two scenarios:
- The main thread is busy doing work.
- The main thread is blocked by another thread or some system resource.


### Main causes for the main thread to be busy
- Proactively doing work 
- Perfoming irrelevant work on the main thread

Main thread service blocks from the main dispatch queue, but it can also service blocks from other queues via dispatch sync.
Anytime a queue dispatch syncs onto another queue, all pending blocks on the other queue have to execute before the newly enqueued one.

Low priority serial dispatch queue, if the main thread dispatch synced a block onto the maintenanceQueue, it will have to wait for all the pending  blocks on that queue to execute before that enqueued block runs

The same way if s block is dispatched onto the main queue from another queue, that block has to execute on the main thread
This holds even if the block is enqueud via a dispatch async

#### Subopotimal API 
Using the wrong API for doing a task, use GPU instead of CPU usage


### Main causes for the main thread to be busy

#### Sync APIs
Synchronous APIs block execution from the time they're called to the time they return.
These should not be used on the main thread if the API does a lot of work or has the potential to block for a long period of time.

Additional point of failure, one such case is if the main thread of an app makes synchronous requests to the network. For those with bad signal, this will take infinetely. 
Which is way suck sync operations should be avoided on the main thread

#### File I/O
Another way to block the main thread is on a system resource. File I/O is on the most used APIs. Latency are dependent on hardware. You should avoid I/O operations on the main thread.
If the main thread tries to **read** at the same time another thread is doing a **write** operation this can leads to a longer read on the main thread.

#### Synchronization
By definition, synchronization primitives can block execution, and we should be careful when syncronizing from the main thread.
The thread it syncronized with can take a long time to release an implicit or explicit lock.

You should avoid using:
```swift
// Example of commom synchronization primitive to look out for.

@synchronized

.lock()
.sync()
.wait()

os_unfair_lock_lock
pthread_mutex_lock
pthread_rwlock_wrlock
pthread_rwlock_rdlock

```

Be aware of semaphore use, as they do not propagate priority and can lenghten a hang due to preemption.

A common anti-pattern is when trying to make asynchronous API act synchronous by waiting on a semaphore. This should always be avoided on the main thread.


#### Getting an unvarying value
You can cause a block on the main thread by doing work, IPC or using system resources to fetch the value of something that doesn't change that often.

#### Resource constraints
- CPU
- Memory
- Storage
- Battery
- Thermal
- Network

Plays a large part when a hang occurs

The high-level cause of hangs is too much work being done on, or on behalf of, the main thread. It should focus to update the UI

## How to diagnose hangs
TimeProfiler shows your application's callstacks over time
The system trace instrument adds more context with data on system calls, VM faults, I/O, as well as inter and intra-process interactions.


## How to eliminate hangs
- Optimize work on the main thread
- Move work off main thread

### Caches
Caches are great way to access data, they're often an in-memory store, but can be persisted to disk, if needed across multiple app invocations.

It's important to have an accurate cache invalidation mechanism to strike a balance between having stale data and constantly updating a cache. This work should happen asynchronously on a secondary dispatch queue to keep the main thread responsive to events.

### Observers
Notification observers are a way to reduce work on the main thread. They allow your app to react to changes in a value or state, without having to do heavy computational work 


### Moving work off the main thread
- All views and viewcontroller should be created, updated on the main thread

### Asynchronous API
- Grand Central Dispatch provides simple mechanisms to move any block of work to another thread, both sychronously and asynchronously.
- Dispatch async that block to another dispatch queue
- You can then DispatchQueue.main back with a completion handler
- GCD enables you to pre-warm computation. By dispatch asyncing a task onto a queue, the task will start executin while the main thread stays free to do other work.


### Understand tradeoffs
Caches use memory, so you should be aware of their size to avoid memory growth, make sure that you have a cache invalidation to remove the stale data.

It's important to know how many times the noticiations will be fired, adding a filter before handling or coalescing multiple notifications will reduce CPU churn.

When using async apis, you need to know if they need to be async. If they usage is crucial for a UI update, as the SO deprioritizes asynced work.

**General tips**
- Use Apple frameworks and APIs
- Perform improvements iteratively
- Use resources carefully
- Set baselines with Xcode organizer
- Identify hang anti patterns
- Triage Instruments and MetricKit



## References
- [Understand and eliminate hangs from your app](https://developer.apple.com/videos/play/wwdc2021/10258/)
- [System Trace in depth](https://developer.apple.com/videos/play/wwdc2016/411)
- [Diagnose Power and Perfomance regressions in your app](https://developer.apple.com/videos/play/wwdc2021/10087)
- [Improving battery life and perfomance](https://developer.apple.com/videos/play/wwdc2019/417)


