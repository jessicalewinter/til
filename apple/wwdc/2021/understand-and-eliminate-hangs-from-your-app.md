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


### Main thread busy
- You shouln't proactively doing work right away
- Perfoming irrelevant work

Anytime a queue


## How to diagnose hangs

## How to eliminate hangs

## References
- (Understand and eliminate hangs from your app)[https://developer.apple.com/videos/play/wwdc2021/10258/]
