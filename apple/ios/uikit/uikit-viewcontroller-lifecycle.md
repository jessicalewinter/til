# UIViewController Lifecycle

## loadView()
This event creates the view that the controller manages. It is only called when the view controller is created programmatically.
You can override this method in order to create your views manually. If you are working with storyboards or nib files, then you do not have to anything with this method and you can ignore it.

## loadViewIfNeeded()
Loads the view controller’s view if it has not already been set.
available from iOS >=9.0

## viewDidLoad()
Called after the view has been loaded.
For view controllers created in code, this is after -loadView.
For view controllers unarchived from a nib, this is after the view is set.
Use this method to initialize setup of the interface

## viewWillAppear(_ animated: Bool)
This method will get called every time the view is about to appear, whether or not the view is already in memory.

## viewWillLayoutSubviews()
Called just before the view controller’s view’s layoutSubviews method is invoked.
This is the first step in the lifecycle where the bounds are finalised. If you are not using constraints or Auto Layout you probably want to update the subviews here.

## viewDidLayoutSubviews()
Called just after the view controller’s view’s layoutSubviews method is invoked.
This event notifies the view controller that the subviews have been setup.

## viewDidAppear(_ animated: Bool)
Called when the view has been fully transitioned onto the screen.

## viewWillDisappear(_ animated: Bool)
This method will get called when the view controller’s view is about to be removed from the view hierarchy.

## viewDidDisappear(_ animated: Bool)
This method will get called when the view controller’s view was removed from the view hierarchy.

## References
- []()
