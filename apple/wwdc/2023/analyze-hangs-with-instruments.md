# Analyze hangs with Instruments
## Find a hang
### Instruments
- At-desk
- Explorative
- All platforms

### On-device Hang Detection
- On-device
- In background
- iOS only

## Event handling and redering loop
Acceptable delays are context dependent

Hang reports are divided in thresholds
- A delay below roughly 100 ms for a insteratcio will feel instantly
- Until 250ms it will feel natural, but above that you're going to start see theslowness to respond
- Migrohangs are delays between 250ms and 500ms
- everything above 500ms it's considered a hang

### Discret interaction

Instant -> < 100ms
Request-Response -> < 500ms

## Event handling and rendering loop
To enable our UI elements to react "instantly", it's important to keep the main thread free from non-UI work.

Hardware -> OS > Event on the main thread -> Render -> Update Display

When we face a delay on our app, it's almost always because the portion on the main thread took too long or something else is still executing on the main thread. When an event is still being executed, we need to wait for it to finish before it can be handled.

Ideally, no work on the main thread should take longer than 100ms. Lower threshold avoid hitched on your app.
 
## Busy main thread
Open Instrumnets to start checking for hangs
Intruments detected an unresponvive main thread and marks the corresponding interval as a pontential hang. There are two main cases for an unresponsive main thread.
- Busy Main Thread: In this case, the main thread will show a bunch of CPU activity
- Blocked Main Thread: Is still waiting for some other work to finish elsewhere.When is blocked, will be little to no CPU activity on the main thread.

Select the main thread and it will open the Profile view, which shows us a call tree of all functions that executed on the main thread during the whole recoriding time.

Double click on the hang to Inspect the stack trace and redirect you to the Profile window. Also, use Call tree to hide external libraries calls

We have a busy main thread meaning the CPU is doing a lot of work.
We have two main causes now: Or this method is runned for a long time or it's just called a lot of time.

If it's called for a long time, we should look for the root cause down on the stack trace.
If it's called for a lot of times, we should look up on top of the stack trace to look for the reasons why is been called so often.

### Time Profiler only samples
- Cannot differentiate between long-running and often-called functions
- Use `os_signpost` or other instruments to measure the precise runtime or how often specific work is performed.

SwiftUI View body instrument you can analyze more in depth what's causing a hang on your app. On the top right corner you can see a plus button, there you can check the Isntruments library. This is the list of all the intruments, the Instruments application has to offer. You can even write your own custom Instruments

Using it, you can see how many times your body was called, which causes a redwar on the SwiftUI view.

Using Grid which causes all the cells to fetch the image from API, but if we use LazyVGrid, it only computed as many views as necessary to fill one screen.

Lots of views in SwiftUI has lazy variants, which only compute as many views as necessary, and this can be an easy way to do less work. However, the eager variants use much less memory

## Blocked Main thread
When your main thread shows almost no CPU usage, that's means it's blocked, waiting for a execution of another thread
To inspect what is causig it, you should add the Thread States intrument.

When using it, you can see that the main thread was blocked for a certain period of time.
The Narrative view tell sus the story of the thread; what it was doing, when, and why.

On the right view, it shows the backtrace of the sync call that blocked the main thread. The function call is displayed as the lead node at the bottom and its call stack is displayed above. 

Static variables are accessed lazily the first timme it's acessed and that happens synchronously. So, this sync call is called on the main thread.

Not at all times a blocked thread will cause a hang. On thic case, the main thread is asleep because there was no user input. From the SO's pesperctive, it's indeed blcoked, but it's just saving resouces by not running when there is nothing to do. As soon a new input comes in, it will wake up and handle it. So, to determina wheter a blocked thread is a responsiveness issue or not, look to the Hangs intrument not the thread states one.

### Fix
- Make singleton property async as well
- Be careful with locks and semaphores in combination with Swift concurrency.

### Clarifications
Blocked Main Thread != Unresponsive Main Thread
High CPU Usage != Unresponsive Main Thread
Unresponsive Main Thread = (Blocked Main Thread or Busy Main Thread)
The isntruments know that, and it will show for potential hangs when the UI become unresponsive

## Wrap-up
- Keep work on the main thread below 100ms
- Determine whether the main thread is busy and blocked
- Hangs can surface synchronously or asynchronously
- To fix hangs, do less work on the main thread
- To fix hangs, move work to the background


## Resources
- [WWDC 2023 session](https://developer.apple.com/videos/play/wwdc2023/10248/)
- [Improve App Responsiveness](https://developer.apple.com/documentation/Xcode/improving-app-responsiveness)
- [Explore UI animation hitches and the render loop](https://developer.apple.com/videos/play/tech-talks/10855)
- [Getting started with Instruments](https://developer.apple.com/videos/play/wwdc2019/411)

