# Interactor layers

By subclassing an interactor, one can create a custom one. This mechanism however, cannot be used to easily extend or modify interactors of different types. In order to do this, one needs to add a Layer (This notion is related to Adorners in WPF) to the interactor. A layer will process events before they reach the interactor. If multiple layers are added to an interactor, events will be processed by the outermost layer first, which is the layer that was added last. If the outermost layer does not handle a particular event, it will be handled by the layer below it, etc.

A layer offers the same event handling virtuals as an interactor (OnMouseDown() etc.). The default implementation of the InteractorLayer class will just invoke the corresponding method of the previous layer, or of the interactor if no previous layer exists. This means that a layer is completely transparent by default, both in terms of appearance and behavior.

In order to create a useful layer, one will need to derive from the base InteractorLayer class and override one or more of its event handling methods. This override may then handle the event differently. Whether or not the handling of the underling layers/interactor are invoked is just a matter of calling base.

For example:

- If one creates a layer that overrides OnPaint() one can change the appearance of the interactor. Marks that are painted before calling base.OnPaint() are painted below the existing appearance. Marks that are painted after calling base.Paint() are painted on top of the existing appearance.
- If one creates a layer that overrides OnMouseClick(), one can change the behavior of an interactor. One may avoid calling base for example, and thus completely hide the mouse click for the underlying interactor. Or, one may just log the occurrence of the click and pass it on unmodified by calling base. Many variations in between are possible.

In addition to overriding the methods for defining the appearance and the behavior, there are a few additional elements that a layer allows to be overridden:

- Rectangle: this property should specify a rectangle that encompasses the interactor and all its layers. As a convenience, the interactor also has a rectangle property that maps onto the rectangle of the outermost layer. If it has no layers this rectangle specifies the rectangle (0, 0, Interactor.Width, Interactor.Height).
- IsOver(x, y): this allows the layer to specify a different shape for the interactor. This may be necessary if the appearance is different.
- HoverCursor: this allows the layer to specify a different cursor when the mouse hovers over the interactors, suggesting different functionality.
