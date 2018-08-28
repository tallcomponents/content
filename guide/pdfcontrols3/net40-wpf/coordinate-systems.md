# Coordinate systems

The PagesViewer control distinguishes these coordinate systems:

- Viewer space
- Document space
- Page space (per page)

Mostly you will be using viewer space and page space.

## Viewer space

Viewer space corresponds to the WPF coordinate system. The origin lies at the top-left corner of the control. The x-axis points to the right, the y-axis points down. Unis are device-independent (1/96 inch).

## Page space

Page space corresponds to the PDF coordinate system and each page has its own page space. The coordinates of PDF elements such as annotations and widgets are defined in their respective page space.

If the page is not rotated, then the origin lies at the bottom-left corner of the mediabox, the x-axis points to the right and the y-axis points up. Units are in points (1/72 inch).

If the PDF page has a cropbox smaller than the mediabox, then the origin lies outside the part of the page as displayed by the pages viewer. This is because the viewer displays the intersection of the cropbox and the mediabox.

If the PDF page has a rotate other than 0, then the axes point in different directions as illustrated below:

## Document space

Document space corresponds to the canvas on which all pages are displayed. At any point, the [viewport](viewport) shows a part or all of this canvas. The x-axis always points to the right, and the y-axis always points down. As in page space, units are in points (1/72 inch).

Properties PagesViewer.ViewportLeft, PagesViewer.ViewportTop, PagesViewer.ViewportWidth and PagesViewer.ViewportHeight return the viewport in document space. 

## Transforming between spaces

Often you want to transform back and forth between coordinate spaces. E.g. if you handle a mouse event and want to know if the mouse hits an annotation, then you want to transform from viewer space to page space. Or if you want to overlay a PDF widget with a custom WPF control, then you want to transform from page space to viewer space.

The following members can be used to transform between spaces:

- PagesViewer.DocumentToViewerTransform
- PagesViewer.ViewerToDocumentTransform

- property PageInteractor.ViewerBounds returns the bounding box of the displayed page in viewer space
- property PageInteractor.DocumentBounds returns the bounding box of the displayed page in document space
- property PageInteractor.ViewerTransform is applied to coordinates in page space to calculate the same coordinate in viewer space


[todo]
