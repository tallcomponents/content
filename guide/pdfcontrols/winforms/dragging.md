# Dragging

Interactors support various types of dragging behavior. Dragging is a more complex operation than ordinary “clicking” as dragging often involves multiple interactors.



## Drag Behavior

When a user starts dragging the mouse over a certain interactor, the DragBehavior property of this interactor determines what will happen next. It can have the following values:
&nbsp;<ul><li>
DragOver: This is the default behavior. Dragging will result in a number of MouseMove events, followed by a MouseClick event and finally, a MouseUp event. These events will all be directed to the interactor in which the mouse button was originally pressed, even if the mouse moves outside the interactor or over one of its children. This behavior is useful in many cases, including selection of text or resizing. Basically, it allows the interactor to “follow” the mouse regardless of where it moves to.</li><li>
DragDrop: This indicates that the interactor can be dragged. In this case, multiple interactors are involved. The source interactor is the one that is being dragged, and the target interactor is the one that it is currently hovering over. Both will received a number of special “Drag” events during dragging. We will have a closer look at this below.</li><li>
DragSelect: If an interactor specifies this behavior, the viewer will drag a “selection rectangle” during dragging. The interactor will first receive a DragSelectMouseDown event, followed by a number of DragSelectMouseMove events, and finally a DragSelectMouseUp event. These events are similar to the ordinary DragOver events, except that they will pass the location and size of the dragged rectangle.</li></ul>&nbsp;
The DragOver behavior is fairly straightforward, so we will not discuss it further. Below, we will have a closer look at the other behavior.



## DragDrop

Once a user starts dragging an interactor with DragDrop behavior, a number of event handlers will be invoked for this source interactor, as well as for any target interactor that it gets dragged over. We will consider these handlers below.



#### Source Events

The following event handlers will get invoked for the source interactor:
&nbsp;<ul><li>
OnDragBegin: This will be invoked when the user starts dragging this interactor. Dragging can be cancelled by removing the interactor from the DraggedInteractors collection of the event. The AllowedEffect property of the event will be set to All. OnDragBegin may change the Effect property to limit this.</li><li>
OnDragEnd: This event will be invoked when the user releases the mouse button. The effect of the Drag can be cancelled by removing the interactor from the DraggedInteractors collection of the event. The Effect property indicates the effect(s) that the target interactor allows.</li><li>
OnPaint: This method will be invoked to draw the original, undragged interactor. By default, this is the same as the normal representation.</li><li>
OnPaintDrag: This method will be invoked to draw a dragged representation of the interactor. The coordinate system will have its origin translated by the same amount as the user has dragged the interactor. By default, this method draws a transparent “ghost” image of the interactor.</li></ul>

#### Target Events

The following event handlers will get invoked for any target interactor that the source interactor gets dragged over.
&nbsp;<ul><li>
OnDragEnter: This will be invoked when the mouse pointer enters the target interactor. This is comparable to OnMouseEnter. The AllowedEffect property of the event contains the value as indicated by the source interactor via OnDragBegin. The Effect property has the value None at this point. If the interactor supports dropping the source interactor within the AllowedEffect possibilities, it should indicate this by modifying the Effects property accordingly. If it fails to do this, it will not be possible to drop the source here.</li><li>
OnDragOver: This will be invoked regularly while the mouse pointer hovers over the target interactor. This is comparable to OnMouseMove. The Effects property will have the value that was left by the last OnDragEnter call, or the previous OnDragOver invocation. OnDragOver may modify this value.</li><li>
OnDragLeave: This will be invoked when the mouse pointer leaves the target interactor. This is comparable to OnMouseLeave.</li><li>
OnDragDrop: This will be invoked when the source interactor gets dropped on this interactor. It will be invoked just after OnDragEnd has been invoked for the source. Normally, OnDragDrop will perform the actual drag-and-drop action.</li></ul>&nbsp;
Please note that a DragDropEvent contains a collection of dragged source interactors. Normally, if a user starts dragging an interactor, it will contain only this interactor. If however the dragged interactor is part of the current selection, all interactors in it will be dragged. Also see the Selection property of the viewer.



## DragSelect

When a user starts dragging over an interactor with DragSelect behavior, the viewer will automatically draw a selection rectangle. While this rectangle gets dragged, the following event handlers may be invoked for the interactor in which dragging started.
&nbsp;<ul><li>
DragSelectMouseDown: This will be invoked at the moment that the mouse starts being dragged. This event will not occur if the mouse just gets clicked. The mouse must actually move while the left mouse button is kept down.</li><li>
DragSelectMouseMove: This will be invoked any time the size of the dragged rectangle changes. The DragSelectEvent will pass a rectangle that provides the size of the selection rectangle.</li><li>
DragSelectMouseUp: This will be invoked when the mouse button gets released.</li></ul>&nbsp;
