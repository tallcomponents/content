# Scrolling and zooming


## Scrolling

The VisibleTop and VisibleLeft properties of the PagesViewer specify which location of the document is located at the top left corner of the viewer. This location is specified in points (a single point is defined as 1/72 inch).


A document is often opened with both VisibleTop and VisibleLeft set to zero, which means that the top left corner of the first page is at (or near, which also depends on the actual layout of the pages, and the margins that are kept around each page) the top left corner of the viewer. Programmatically, one can scroll down by increasing the VisibleTop value, and to the right by increasing the VisibleLeft value. If a user scrolls the document (e.g. via the scroll bars), the VisibleTop and VisibleLeft properties get updated automatically.


The VisibleWidth and VisibleHeight values of the viewer specify how much of the document is visible. These too are specified in points. One cannot set these values, as they get derived from the actual width and height of the control (which of course, are settable).



## Zooming

The size of a PDF page is also given in points. This provides the basis for the definition of the ZoomFactor property of the DocumentViewer class. A zoom factor of 1 means that a single point in the document (1/72 inch) corresponds to a single pixel. As screens historically had a resolution of 72 pixels per inch, this is also referred to as “actual size”.


Setting the zoom factor to 2, means that the document gets “blown up” by a factor of 2 compared to its actual size. A single point then occupies a square of 2x2 pixels. Likewise, a zoom factor if 0.5 means that the document gets shrunk by a factor of 2 compared to its actual size. A single pixel then represents a square of 2x2 points.


In principle, zooming in and out can be done by specifying the appropriate zoom factor.


Without further measures however, changing the zoom factor will just keep the same document position at the top left corner of the viewer, and zoom in or out around that position. To zoom into other locations, one needs to set the VisibleTop and VisibleLeft properties of the viewer as well.



## Zoom Methods

To make zooming easy, the DocumentViewer, the PagesViewer and the StandardPagesViewer define a number of zooming methods.



#### Document Viewer

The DocumentViewer defines the following zoom methods.
&nbsp;<ul><li>
ZoomTo(zoomfactor, x, y): sets the zoom factor to the given value, and centers the view around the given document coordinate.</li><li>
ZoomToRectangle(rectangle): zoom to the specified rectangle. The rectangle is given in document coordinates. The viewer will not change the width and height of the control, but it will zoom as far in as possible, in such a way that all of the rectangle will be shown.</li><li>
ZoomToWidth: sets the zoom factor so that the width of the document fits the visible width of the viewer.</li></ul>

#### Pages-Viewer

Additionally, the PagesViewer defines a ZoomToFitPage method that will set the zoom factor and the visible area such that the current page is completely visible, and as large as possible.



#### StandardPagesViewer

The StandardPagesViewer defines a ZoomFactors collection, as well as a number of convenient extra zoom methods:
&nbsp;<ul><li>
ZoomToActualSize: sets the zoom factor to 1.</li><li>
ZoomIn(x,y): sets the zoom factor to the next higher value in the ZoomFactors collection, setting the center of the visible area at the specified document location. The zoom factor will never be set to a higher values than the MaxZoom property.</li><li>
ZoomOut(x,y): sets the zoom factor to the next lower value in the ZoomFactors collection, setting the center of the visible area at the specified document location. The zoom factor will never be set to a lower value than the MinZoom property.</li></ul>&nbsp;
&nbsp;<table><tr><th>![Note](media/AlertNote.png) Note</th></tr><tr><td>The ZoomFactors collection is ordered from small to large. Adding a factor will automatically place it at the right position in this order. Inserting a factor must be done at the right position, or it will throw an exception.</td></tr></table>

## Zoom Modes

Next to these zoom methods, the StandardPagesViewer offers a number of zoom modes. These are implemented in terms of the zoom methods above, and keep the pages of a document zoomed in a particular way.


