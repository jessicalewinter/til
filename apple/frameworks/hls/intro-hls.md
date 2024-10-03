## An Introduction to HLS

HLS tags

## Redudant Streams
AVPlayer will automatically switch between streams not only when network conditions change, but also when we have failure fallback. If the client is unable to reload a stream(due to an 4xx or 5xx error, for example), the client will attempt another altenete stream with the highest bandwidth that the current network connection supports. For more information about this behavior, take a look at this [here](https://developer.apple.com/library/archive/documentation/NetworkingInternet/Conceptual/StreamingMediaGuide/UsingHTTPLiveStreaming/UsingHTTPLiveStreaming.html#//apple_ref/doc/uid/TP40008332-CH102-SW22)


## References
- [Apple | HTTP Live Streaming](https://developer.apple.com/streaming/)
- [What's new in HTTP Live Streaming | WWDC 2024](https://developer.apple.com/streaming/Whats-new-HLS.pdf)
