# Layout managers

Next to positioning the interactors explicitly, it is possible to assign a Layout Manager to the Layout property of any interactor. Each layout manager automatically positions the children of the interactor that it has been attached to.


As indicated earlier, each viewer also has a layout manager to position the pages of a document. This layout manager is actually a reference to the layout manager of the root interactor. It exists in the viewer not only as a convenience, but also because the standard edition has no access to interactors.


Please note that a layout manager will actively update the Transform properties of the child interactors. If one needs to set these explicitly, please make sure to set the layout manager to null, or use a layout manager that takes into account the explicit positioning of the children.



## Standard Layout Managers

Next to the GridLayoutManager and the AutoColumnGridLayoutManager, we provide 2 special-purpose layout managers:
&nbsp;<ul><li>
PageLayoutManager: this layout manager is typically assigned to a DocumentInteractor. It first resets the Transform of each page and then makes sure that all page interactors are rotated according to their orientation.</li><li>
AnnotationLayoutManager: this layout manager is typically assigned to a PageInteractor. It makes sure that all annotation interactors are placed in the coordinate system of their PageInteractor, according to their location in the PDF page. So basically, this layout manager translates coordinate in PDF page space (origin at lower left corner) to PageInteractor space (origin at top left corner).</li></ul>&nbsp;
These layout managers do not take into account the existing Transform of an interactor. So, if these layout managers are attached to an interactor of which the transform was changed explicitly, the layout manager will at some point overwrite this transform.



## Custom Layout Managers

The professional edition allows the construction of custom layout managers. Each layout manager has an Arrange() method that can be overridden to define a different layout. The Arrange() method gets two arguments:
&nbsp;<ul><li>
Interactors: this is the collection of interactors that must be laid out. Their Transform contains information about their current position.</li><li>
A layout context. This provides information about the area that the passed interactors must be placed in. Currently, it specifies a) the rectangle that the parent occupies, and b) a rectangle that indicates the visible area. Both are specified in the parent coordinate system. Future versions may include more extensive information.</li></ul>&nbsp;
If one wants to create a custom layout manager, one just needs to provide an arrange method that updates the Transform property of the interactors.



## Combining Layout Managers

In some cases, it is useful to be able to construct new layout managers based on existing ones. The base LayoutManager allows this by means of a constructor that takes another layout manager for its argument. The base Arrange method will invoke the arrange method of the passed layout manager. As a convention, derived classes that support argument layout managers should first invoke base.Arrange(), and then perform their own layout. In this way, both layout steps can be combined.


The GridLayoutManager employs this. For page layouts, one will typically pass the PageLayoutManager as an argument to the GridLayoutManager. In this way, each page will first be rotated so that it conforms to the specified page orientation, and then, the resulting interactor will be placed in a grid.



## Automatic Layout

Please be aware that the layout manager can be invoked at unpredictable times. It will be invoked at some point shortly after one of the following situations has occurred:
&nbsp;<ul><li>
After invoking the Invalidate method of the layout manager.</li><li>
After a new layout manager has been assigned.</li><li>
After children have been added, removed, or reordered.</li><li>
For document interactors, after a property of one of its pages changes, except when this property is known to have no influence on the size or orientation of the page.</li><li>
For page interactors, after a property of one of its annotations changes, except when this property is known to have no influence on the size or orientation of the annotation.</li></ul>&nbsp;
The arrange method will not be invoked after changing the transform of a child interactor. In most cases, the transforms will either be managed by a layout manager, or they will be set explicitly. If you want to force a layout step after changing a transform, just call invalidate in the layout manager (and make sure that you use a layout manager that does not immediately overwrite the transform).


Note also that the arrange method will only be invoked automatically if a property of a direct child has changed. It will not be invoked when interactors lower down change, nor when the parent interactor itself changes, or any interactor higher up for that matter. The reason for this is simple: we try to avoid complexity. If we were to consider additional levels, calling arrange itself would not only cause new layout steps to be started deeper down, but also higher up. This leads to a system that is difficult to understand and control (As well as being very inefficient).


For many situations, this simple approach works well. In case one does want to arrange layout in more fancy ways, one can do so by triggering layout steps explicitly in a particular order for several levels, possibly via custom implementations of the Arrange method. In this way, the programmer remains in control of the system.


The GridLayoutManager employs this. For page layouts, one will typically pass the PageLayoutManager as an argument to the GridLayoutManager. In this way, each page will first be rotated so that it conforms to the specified page orientation, and then, the resulting interactor will be placed in a grid.


