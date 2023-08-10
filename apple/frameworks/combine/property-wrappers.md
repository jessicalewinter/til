# Combine property wrappers

## @Published property wrapper
Combine's @Published is a publisher that's wrapped by a property wrapper. This gives it the $ prefixed property and other features that SwiftUI relies on to work.
An @Published property is also not like an AnyPublisher at all. @Published always has Never as its failure type, AnyPublisher can have other failure cases.

@Published has a sense of state/ a current value which isn't the case for AnyPublisher. A CurrentValueSubject would come closest but that wouldn't work because @Published can be used as a binding which isn't possible for CurrentValueSubject. An important difference is that SwiftUI can assign new values to an @Published property directly (isLoggedIn = true would trigger a change here).

