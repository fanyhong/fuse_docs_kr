원본: https://www.fusetools.com/docs/full-ux-class-reference

## 전체 UX 마크 업 클래스 참조 ##

이것은 UX 마크업에서 사용가능하도록 Fuse 와 함께 제공되는 클래스의 전체 목록입니다. 이러한 각 클래스들은 마크업안에서 태그의 이름으로 인스턴스화 될 수 있습니다.

> Fuse 에는 고급 사용 사례에서 Uno 프로그래머가 사용할 수 있는 많은 클래스들이 포함되어 있습니다. Native Interop 섹션을 참조하십시오.

## Application ##

`App` 클래스는 여러분 응용 프로그램의 루트를 표시합니다.

## Nodes ##

Activated UX
Active whenever a page becomes active. 
ActivatingAnimation UX
Specifies an animation for an element that's becoming active. 
AddingAnimation UX
Triggers when the element is added to the visual tree. 
AlternateRoot UX
Allows placing a node in a different place in the UX tree than the location of this behavior, while keeping the data context from this behavior.
Android UX
Triggers if run on an Android device 
Attractor<T> UX
Animates a property to a target value using a physics-like attraction simulation. 
BackButton UX
Displays a backbutton 
Blur UX
Applies a gaussian blur to an Element.
BottomBarBackground UX
Compensates for space taken up by the keyboard and other OS-specific elements at the bottom of the screen. 
BottomFrameBackground UX
Busy UX
Marks a UX node as busy. 
Button UX
Displays a button 
ButtonBase UX
Baseclass for buttons 
Circle UX
Displays a circle 
CircularRangeBehavior UX
Helper class for implementing circular RangeControls 
Clicked UX
Triggers when a pointer is clicked on a Visual. 
ClientPanel UX
ClientPanel compensates for space taken up by the on-screen keyboard, status bar, and other OS-specific elements at the top and bottom edges of the screen. 
Closure UX
Captures the named UX objects and dependencies in the scope and sends them to a script event when ready.
Completed UX
Pulses when the busy status of a node is cleared. 
Container UX
A panel that places children in a dedicated Subtree visual, allowing you to create custom container. 
ContainingText UX
DEPRECATED: Use WhileContainsText instead
ContentControl UX
Content controls display a single visual child. 
Deactivated UX
Active whenever a page becomes inactive. 
DeactivatingAnimation UX
Specifies an animation for an element that's becoming inactive. 
Deferred UX
Defers the creation of nodes to improve initialization time. 
Desaturate UX
Desaturates an Element.
Deselected UX
Fired when the Selectable is removed from the Selection.
DirectNavigation

## Navigation Order ##

DockPanel UX
Lays out its children by docking them to the different sides, one after the other. 
DoubleClicked UX
Triggers when a pointer is double-clicked on a Visual. 
DoubleTapped UX
Triggers when a pointer is double-tapped on a Visual. 
Draggable UX
DropShadow UX
DropShadow applies an underlying shadow to an Element.
Each UX
Displays a collection of objects using the given template(s) for each item. 
EdgeNavigation UX
EdgeNavigator UX
EdgeSwipeAnimation UX
DEPRECATED: Use SwipeGesture with EdgeNavigator instead
Ellipse UX
Displays an ellipse 
EnteredForceField UX
Triggers as a draggable element enters the force field. 
EnteringAnimation UX
Specifies an animation for a page that is in front of the active one. 
ExitedForceField UX
Triggers as a draggable element leaves the force field. 
ExitingAnimation UX
Specifies an animation for a page that is behind the active page. 
GraphicsView UX
A native view that hosts graphics-rendered UI controls. 
Grid UX
Lays out children in a grid formation. 
Halftone UX
Applies a classic halftone effect to an Element.
HierarchicalNavigation UX
Image UX
Displays an Image 
InForceFieldAnimation UX
Animates a draggable element depending on how close it is to a point attractor 
Instance UX
Creates and inserts an instance of the given template(s).
InteractionCompleted UX
Triggers when an interaction completes. 
iOS UX
Triggers if run on an iOS device 
JavaScript UX
The JavaScript tag is used to run JavaScript and assigns its module.export as data context for the parent visual. 
KeepFocusInView UX
KeepInView UX
LayoutAnimation UX
Triggers when the layout of an element changes 
LayoutControl UX
Layout controls perform layout of the children. 
LinearNavigation UX
Navigation Order 

LinearRangeBehavior UX
Common linear sliding behaviour used for implementing a RangeControl. 
LongPressed UX
Triggers when a pointer is held down for a period of time. 
MapMarker UX
Adds a map marker to a MapView 
MapView UX
Displays a native map view. 
Mask UX
Masks an Element to an image.
Match UX
Compares a value with a set of constants, and activates/deactivates visual trees associated with those constants. 
MultiLayout UX
MultiLayoutPanel UX
Allows you to move Elements between different layouts using the Placeholder class. 
NativeViewHost UX
Creates a layer of native Controls on top of a GraphicsView. Controls in its subtree will be mapped to the native controls provided by the OS. 
NavigationBar UX
Navigator UX
General-purpose navigation container with on-demand instantiation and recycling of pages. 
NavigatorSwipe UX
Adds navigation to a target page with a swipe gesture. 
NodeGroup UX
Allows creating a class that contains several nodes and resources that are added directly to their Parent, as though included directly. 
Number UX
Deprecated, for backwards compatibility
OnBackButton UX
Triggers when the back-button is pressed 
OnKeyPress UX
Triggers when a specific key is pressed 
OnUserEvent UX
Triggers when a UserEvent is raised. 
Page UX
Represents a page that participates in navigation. 
PageBeginLoading UX
Triggers when a WebView begins loading a page 
PageControl UX
Provides standard transitions, user interaction, and page handling for a basic linear navigation. 
PageIndicator UX
PageIndicatorDot UX
PageLoaded UX
Triggers when a WebView finishes loading a page 
PageResourceBinding<T> UX
PageView UX
A specialized Navigator that has no standard transitions.
Panel UX
Performs the default layout on the children, where all children get all available space. 
PanGesture UX
A TransformGesture that provides panning (2D translation). 
Path UX
Placeholder UX
PointAttractor UX
Pressed UX
Triggers when a pointer is pressed on a visual. As opposed to Clicked or Tapped, this trigger triggers immediately when a pointer is pressed on the visual. It does not wait for a pointer release or minimum amount of press time.
ProgressAnimation UX
Triggers when a Slider or other compatible control changes its value. 
PullToReload UX
Helps you create a "pull to reload" interaction with a ScrollView. 
RangeAdapter<T> UX
Changes the range of an animation. 
RangeAnimation UX
Animates using a value clamped between a minimum and a maximum as progress. 
RangeControl UX
Baseclass for controls that contains a range value 
RangeControl2D UX
Rectangle UX
Displays a rectangle. 
RegularPolygon UX
Released UX
Triggers when a pointer is released on a Visual. 
RemovingAnimation UX
Animates when the parent element is removed 
ResourceBool UX
ResourceFloat UX
ResourceFloat2 UX
ResourceFloat3 UX
ResourceFloat4 UX
ResourceObject UX
ResourceString UX
RightFrameBackground UX
RootViewport UX
RotateGesture UX
A TransformGesture that provides rotation. 
Rotation UX
Rotates the element by the degrees specified. 
Router UX
Manages routing and navigation history for part or all of a Fuse app. 
Scaling UX
Enlarges or shrinks the element by the factor specified. 
Scrolled UX
Triggers when the ScrollView is scrolled to within a specified region. 
Scroller UX
ScrollingAnimation UX
Animates over a given scroll range. 
ScrollView UX
Used to navigate contents that are larger than the available size. 
ScrollViewBase UX
A ScrollView is a control that allows scrolling over the content. It only accepts a single child, from which the size of the scrollable area is calculated. That child can be a single element or a UX tree of controls. 
Select UX
Deprecated.
Selectable UX
Selectable makes a Visual selectable. Selectable visuals are what can be selected in a Selection control. 
Selected UX
Fired when the Selectable is assed to the Selection.
Selection UX
Selection is used to create a selection control, such as an item list, radio buttons, or picker. The Selection itself defines the selection, managing the high-level behaviour and tracking the current value. A variety of Selectable objects define which items can be selected. 
Shadow UX
Draws a shadow behind an element. 
Shear UX
Applies a shear to the visual (skews it). If you wish to animate the shear use a Skew animator instead. 
Slider UX
Displays a slider 
Spring UX
StackPanel UX
Stacks children vertically (default) or horizontally. 
Star UX
Displays a star 
State UX
Acts as a normal Trigger, but is activated by its containing StateGroup. 
StateGroup UX
Used to group a set of States together and switch between them. 
StatusBarBackground UX
Compensates for space taken up by the status bar. 
StatusBarConfig UX
Configures the appearance of the status bar on Android. 
StatusBarConfig UX
Configures the appearance of the status bar on iOS. 
Swiped UX
Pulse trigger that activates when a swipe has occurred. 
SwipeGesture UX
Recognizes a swipe (the movement of a pointer in a given direction). 
SwipeNavigate UX
SwipingAnimation UX
A trigger that maps the progress of a SwipeGesture to a series of animations. 
Switch UX
Displays a switch 
Tapped UX
Triggers when a pointer is tapped on a Visual. 
Text UX
Displays a block of text. 
TextBlock UX
Deprecated, for backwards compatibility
TextBox UX
A TextInput with a default look and feel. 
TextInput UX
Single-line text input control. 
TextInputActionTriggered UX
Trigger for input action 
TextView UX
Multi-line text editor. 
Timeline UX
Groups several animations together 
ToggleControl UX
Panel that contains a toggleable value 
TopFrameBackground UX
Transition UX
Controls the animations for page-to-page transitions in a Navigator. 
Translation UX
Represents a linear offset in space. For animated translation, consider using a Move animator instead of animating the properties of this class.
UserEvent UX
Defines a custom event that can be raised by a component and responded to by a user of that component. 
Video UX
Displays a video. 
Viewbox UX
Forces the content (by scaling) to fit inside the available space. 
WebView UX
Displays web content natively on android and iOS. 
WhileActive UX
Animates while the page is active. 
WhileBusy UX
A trigger that is active whenever a sibling or parent is marked as busy. 
WhileCanGoBack UX
Active whenever navigating backward is possible. 
WhileCanGoForward UX
Active whenever navigating forward is possible. 
WhileCompleted UX
Active while the Video is completed. 
WhileContainsText UX
Active while the surrounding context contains text. 
WhileCount UX
Active when the number of items in a collection fulfills some criteria. 
WhileDisabled UX
Active while the IsEnabled property of its containing element is False.
WhileDragging UX
Active while the element is being dragged.
WhileEmpty UX
Active when the number of items in a collection is 0. 
WhileEnabled UX
Active while the IsEnabled property of its containing element is True. 
WhileFailed UX
Active while the context has failed. 
WhileFalse UX
A trigger that is active while its Value property is false. 
WhileFloat UX
Active when the float Value fulfills some criteria.
WhileFocused UX
Active whenever its containing element is in focus.
WhileFocusWithin UX
Active whenever a child of its containing element is in focus.
WhileHovering UX
Active while a pointer is within the bounds of its containing element. 
WhileInactive UX
Animates while the page is inactive. 
WhileInEnterState UX
A directional version of WhileInactive. 
WhileInExitState UX
A directional version of WhileInactive. 
WhileInteracting UX
Active while the user is interacting with the surrounding element. 
WhileKeyboardVisible UX
Active whenever the on-screen keyboard is visible.
WhileLoading UX
Active while a resource in the surrounding context is loading. 
WhileNavigating UX
Active while the user is currently navigating between two pages. 
WhileNotEmpty UX
Active when the number of items in a collection is greater than 0. 
WhileNotFocused UX
Active whenever its containing element is not in focus. 
WhilePageActive UX
Is active while a page, optionally matching given criteria, is active in the navigation. 
WhilePageLoading UX
A trigger that is active while its parent WebView is loading. 
WhilePaused UX
Active while the Video is paused. 
WhilePlaying UX
Active while the Video is playing. 
WhilePressed UX
Active while at least one pointer is pressed on a visual. 
WhileScrollable UX
Active when a ScrollView can be scrolled. 
WhileScrolled UX
Is active while the ScrollView is scrolled within a given region. 
WhileSelected UX
This trigger is active while the Selectable is currently selected in the Selection 
WhileString UX
Activate when the condition on the string value is true
WhileSwipeActive UX
Active whenever an Active-type SwipeGesture has been swiped to the active state. 
WhileSwiping UX
Is active while a swiping gesture is in progress. 
WhileTrue UX
A trigger that is active while its Value property is true. 
WhileVisible UX
Active when the parent element is visible.
WhileVisibleInScrollView UX
Active while an element is positioned within the visible area of the ScrollView. 
WhileWindowLandscape UX
Active when the app's viewport width is larger than its height. 
WhileWindowPortrait UX
Active when the app's viewport height is larger than or equal to its width. 
WhileWindowSize UX
Active while the size of the app's viewport fulfills some given constraints. 
With UX
Represents a scope in which the current data context is narrowed down. 
WrapPanel UX
Lays out children one after the other in a given orientation and wraps around whenever it reaches the end. 
ZoomGesture UX
A TransformGesture that provides zooming.

## Transforms ##

Rotation UX
Rotates the element by the degrees specified. 
Scaling UX
Enlarges or shrinks the element by the factor specified. 
Shear UX
Applies a shear to the visual (skews it). If you wish to animate the shear use a Skew animator instead. 
Translation UX
Represents a linear offset in space. For animated translation, consider using a Move animator instead of animating the properties of this class.

## Effect classes ##

## Special Resources ##

Font
FileImageSource UX
Specifies an image file as a data source to be displayed by an Image element. 
HttpImageSource UX
Provides an image fetched via HTTP which can be displayed by the Image control. 
MultiDensityImageSource UX
Used to specify multiple image sources that an Image element can display at different pixel densities. 
TextureImageSource UX
Specifies a texture2D object to be displayed by an Image element. 
