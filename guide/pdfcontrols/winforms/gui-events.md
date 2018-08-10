# GUI Events

As we have seen above, an interaction provides its appearance and behavior by implementing virtual methods – like OnMouseDown - that get an instance of EventArgs as their argument. This is consistent with the way that WinForms and WPF basically work.

There is also an important difference. In PDFControls.NET, the base implementations of these methods will not raise any events (These are called “bubbled” events in WPF, as they appear to “bubble up”. Bubbled events are often problematic, as they allow an event to be modified by an external party during the execution of code – like OnMouseDown - that is actually handling the event already. We avoid this issue completely. Please note that it is very easy to reintroduce these events in case one does need them. One merely needs to create a custom interactor that introduces such events. In that case, the programmer also has complete knowledge over the moment that the event is raised). As a result, it is also not mandatory to call base when such methods are invoked. Not invoking base is actually a good way to turn off the corresponding base functionality.

In essence, the virtuals in the interactors are all about event handling, not about event raising.

## Preview Events

This does not mean that interactors do not raise any events at all. They will raise preview events, similar to the preview (or tunneled) events in WPF.

These types of events do not originate at a particular interactor, but they arise “from outside”. In essence the user generates these events by clicking the mouse, by entering text, and by other means to interact with the computer. They subsequently end up at a viewer, which then tunnels the event down to the appropriate interactor, starting with the root interactor (the document interactor).

If a user clicks the mouse over an annotation, then:

- The viewer will start by raising a MouseDown preview event.
- Next, the DocumentInteractor will raise a MouseDown preview event.
- The event will then be tunneled down to the PageInteractor in which the annotation resides. This page interactor will subsequently raise its own MouseDown preview event.
- And finally, the event will be tunneled to the proper AnnotationInteractor. This will then raise a MouseDown preview event as well. After this event has been raised, the annotation interactor will invoke OnMouseDown in order to handle the event.

This means that preview events allow an outsider to influence the event data before it actually gets handled by the appropriate interactor.

In addition, preview events allow the event to be cancelled. If the cancel flag of a preview event gets set, the event will not be tunneled any further. Instead, the last interactor that raised an uncancelled event will get to handle it.

Using the example above: if one cancels the preview event that got raised by the AnnotationInteractor, the event will not be processed by the AnnotationInteractor, but by the PageInteractor. This means that the OnMouseDown method of the page interactor will be invoked instead.

Likewise, if one cancels the preview event that got raised by the PageInteractor, the OnMouseDown() method of the DocumentViewer will be invoked. The AnnotationInteractor will not even generate a preview event then.

## Interactor Events

Given the event processing mechanism that has been described above, there are various types of events that an interactor may get:

### PaintEventArgs

This type derives from System.Windows.Forms.PaintEventArgs, and extends it with a RenderSettings property. It is passed to OnPaint(), and related virtuals.

### MouseEventArgs

This type derives from System.Window.Forms.MouseEventArgs. It redefines X and Y, and Location as types that are based on doubles to avoid rounding errors. This is needed because the mouse events will always be localized to the coordinate system of a particular interactor before they are passed, and this implies that integer positions are not sufficient. This event is passed to various mouse handling virtuals.

### KeyboardEventArgs

This type derives from System.Windows.Forms.KeyEventArgs. It adds a Char and a String property:

- The String property contains the culture-specific text of the keyboard event. In most cases this is the value that you will need to access as it represents the actual text that was entered. Pressing the decimal separator on the keypad for example will provide the separator of the current input language. If text is pasted, it will provide the entire text.
- The Char property is only applicable if the KeyBoardEvent represents an actual key press (or release). It corresponds to the actual character that was entered, i.e. shift + ‘key a’ provides ‘A’ as the Char value. Basically, this corresponds to the KeyChar value of System.Windows.Forms.KeyPressEventArgs. Char holds a null value ('\0') for keys that do not have a character representation (such as the arrow keys) (_windows does not even generate a KeyPress event for these keys_), and if the KeyBoardEvent is not the result of an actual key press (e.g. when pasting some text).

### DragDropEventArgs

This type derives from System.Windows.Form.DragEventArgs. It redefines X and Y as doubles for the same reason as MouseEventArgs. In addition it defines the following properties:

- DX: the distance that an element (interactor) was dragged in the X direction, in screen coordinates.
- DY: the distance that an element (interactor) was dragged in the Y direction, in screen coordinates.
- DraggedInteractors: a collection of dragged interactors, if any.

### DragSelectEventArgs

This event will be raised when the user drags a selection rectangle over some interactor. This type is derived from System.Windows.Forms.EventArgs, as WinForms has no specific event type with similar functionality. It defines a single property:

- Rectangle: this is the bounding rectangle of the dragged area, specified in the coordinate system of the interactor that receives the event.

## Other Events

A few other event types have been defined in the TallComponents.Interaction.WinForms.Events namespace. These events generally have to do with interaction, but they are not passed to one of the interactor virtuals that handle GUI events.

### Bookmark events

There exist various bookmark events. These are only raised by the Bookmarks viewer. Please see the event description of the BookmarksViewer in the reference documentation.

### Unhandled Exceptions

PDFControls.NET regularly executes code that is not invoked by user code directly, but as a result of some external event (like a mouse click) that triggers the execution of particular code.

PDFControls.NET tries to avoid throwing exceptions in such code, but it is always possible that an exception occurs that we cannot handle, for example if GDI throws some exception during the execution of OnPaint(). In order to allow clients to handle such situations, they can subscribe to the UnhandledException event of the DocumentViewer. If subscribed to, each unhandled exception will be transformed into an UnHandledException event. The handler may then choose to handle it, or throw an exception after all.

## Advanced: WinForms Viewer Events

Each viewer is actually a WinForms control, and as such, it will invoke various WinForms control virtuals - like OnMouseDown -, including raising the corresponding WinForms events. As always, it is possible to attach your own handlers to these, but in order to this effectively it is important to understand the relation between these WinForms events and the interactor events that the interactors receive.

### Mouse Events

In order to be able to handle mouse events, our viewer has overridden various event handlers of the base control, like for example OnMouseDown(). If a WinForms mouse event arrives at a viewer control, this event handler will get invoked. For mouse events, our viewer subclass will first call the base handler. (base.OnMouseDown()). The latter will raise the corresponding WinForms event (MouseDown). After this event has been handled, the event will be passed down the interactor tree as explained above. This leads to a number of Preview events, and finally the invocation of the event handler (Interactor.OnMouseDown).

This means that mouse events can be inspected and handled at the viewer level before they reach any of the interactors in the viewer. If it is necessary to avoid subsequent processing by the interactors, one may subscribe to one or more preview events of the viewer and cancel them. Or alternatively, one could override the viewer level event handler (OnMouseDown()) and avoid calling base so that the viewer functionality is not triggered at all (Or call base.base.OnMouseDown() so that the viewer implementation is skipped, and the WinForms event is still fired). In general however it is probably better to avoid cancelling mouse events at the viewer level, and insert modified handlers at the appropriate interactor level. It is also possible to let interactors ignore events by setting their IsInteractive flag to false.

This means that mouse events can be inspected and handled at the viewer level before they reach any of the interactors in the viewer. If it is necessary to avoid subsequent processing by the interactors, one may subscribe to one or more preview events of the viewer and cancel them. Or alternatively, one could override the viewer level event handler (OnMouseDown()) and avoid calling base so that the viewer functionality is not triggered at all1. In general however it is probably better to avoid cancelling mouse events at the viewer level, and insert modified handlers at the appropriate interactor level. It is also possible to let interactors ignore events by setting their IsInteractive flag to false.

If Preview events must be cancelled, please note that there may not be a trivial relation between the raised Preview events and the original WinForms event that triggered them:

- MouseDown: this WinForms event will always lead to a PreviewMouseDown event.
- MouseMove: this WinForms event will ordinarily lead to a PreviewMouseMove event. If the mouse is being dragged however, it may trigger PreviewSelectMouseDown, PreviewSelectMouseMove, and PreviewDragBegin events, depending on the DragBehavior property of the interactor. In addition, this event may trigger various Enter and Leave events.
- MouseUp: this WinForms event may trigger a PreviewMouseUp or PreviewDragSelectMouseUp event.
- DragDrop: this WinForms event will trigger a PreviewDragDrop event.
- DragOver: this WinForms event will trigger a PreviewDragOver event.
- DragEnter/DragLeave: these WinForms events do not trigger any preview events, but they are overridden by the viewer to support dragging object into or out of the viewer.

Note that the mouse preview events of the Viewer are raised after the WinForms event was raised that triggered it.

### Keyboard Events

Keyboard event handling is largely similar to mouse event Handling. If a WinForms keyboard event arrives at a viewer control, first the corresponding WinForms event handler will be invoked, say OnKeyDown(), which will then first call the base handler (base.OnKeyDown()). The latter will raise the corresponding WinForms key event (KeyDown). Hereafter the viewer will dispatch the event to the interactor that has the focus.

There is also a difference with mouse event handling: after the WinForms key event has been raised, the viewer will inspect the handled flag of the event. If it has been set, it will not pass the event to the focused interactor. In this way further processing of keyboard events can easily be cancelled.

### Paint Events

The paint event gets handled differently than keyboard events and mouse events. If the OnPaint method of the viewer gets invoked, it will first pass the event to the interactors, so that they get drawn. After this has completed, it will invoke the base handler (base.OnPaint()). The latter will raise the WinForms Paint event.

In this way, any subscriber will be able to easily paint additional marks over the interactors.