---
title: Architecture
layout: default
---

# Segue types

| Segue type | Behavior |
|  --------- | --------- |
| Show (Push) | This segue displays the new content using the showViewController:sender: method of the target view controller. For most view controllers, this segue presents the new content modally over the source view controller. Some view controllers specifically override the method and use it to implement different behaviors. For example, a navigation controller pushes the new view controller onto its navigation stack. UIKit uses the targetViewControllerForAction:sender: method to locate the source view controller.|
| Show Detail (Replace) | This segue displays the new content using the showDetailViewController:sender: method of the target view controller. This segue is relevant only for view controllers embedded inside a UISplitViewController object. With this segue, a split view controller replaces its second child view controller (the detail controller) with the new content. Most other view controllers present the new content modally. UIKit uses the targetViewControllerForAction:sender: method to locate the source view controller. |
| Present Modally | This segue displays the view controller modally using the specified presentation and transition styles. The view controller that defines the appropriate presentation context handles the actual presentation.|
| Present as Popover | In a horizontally regular environment, the view controller appears in a popover. In a horizontally compact environment, the view controller is displayed using a full-screen modal presentation.|

# Methods

## **showDetailViewController(_:sender:)**

```swift
func showDetailViewController(_ vc: UIViewController, sender: Any?)
```

### Discussion
You use this method to decouple the need to display a view controller from the process of actually presenting that view controller onscreen. Using this method, a view controller does not need to know whether it is embedded inside a navigation controller or split-view controller. It calls the same method for both. In a regular environment, the UISplitViewController class overrides this method and installs vc as its detail view controller; in a compact environment, the split view controller’s implementation of this method calls show(_:sender:) instead.

The default implementation of this method calls the targetViewController(forAction:sender:) method to locate an object in the view controller hierarchy that overrides this method. It then calls the method on that target object, which displays the view controller in an appropriate way. If the targetViewController(forAction:sender:) method returns nil, this method uses the window’s root view controller to present vc modally.

You can override this method in custom view controllers to display vc yourself. Use this method to display vc in a secondary context. For example, a container view controller might use this method to replace its secondary child. Your implementation should adapt its behavior for both regular and compact environments.

When use this method the new view controller, that is showed on the screen, does not have the *"Back button"* on the navigation bar :cry:
