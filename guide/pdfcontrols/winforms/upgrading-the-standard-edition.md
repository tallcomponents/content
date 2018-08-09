# Upgrading the Standard Edition


## Account for removed classes, enumerations and delegates.

A number of classes, enumerations and delegates that were previously in the namespace TallComponents.PDF.ReaderControls have been removed altogether. The functionality that these classes provided has either been taken over by de new object model, or can be implemented in it. The table below lists all classes that have been removed. For simplicity, we have left out the event “handler” classes.



## Document

The following properties have been moved to the new DocumentInfo class:
&nbsp;<ul><li>
Author</li><li>
Creator</li><li>
Title</li><li>
Subject</li><li>
Keywords</li></ul>

## PagesViewer

This object incorporates the most changes, and will be the most complex to upgrade. Much functionality that previously existed in the PagesViewer class has moved to the StandardPagesViewer class. The PagesViewer class itself has been reduced to a more bare-bones version that is now more suitable for creating custom viewers (of which the StandardPagesViewer is basically an example).


The sections below will only consider methods and properties that the StandardPagesViewer does not provide.



#### BottomMargin, TopMargin, LeftMargin and RightMargi

These properties no longer exist. Instead, the StandardPagesViewer defines a DocumentMargins property, which in turn holds the values for Bottom, Top, Left and Right. These values are no longer integers, but doubles.


The StandardPagesViewer derives these margins from the margins of the active layout manager. Currently this is only supported for instances of the GridLayoutManager. For an example of changing the margins of the layout manager please see the Bookviewer sample.



#### ClientToDocument and DocumentToClient

These transformations are no longer present. Instead, please use the DocumentToClientTransform or its inverse. The following code shows how to implement the 1.1 calls on top of this transform.


```
public System.Drawing.PointF ClientToDocument(System.Drawing.PointF point)
{
  Point result = new Point(point.X, point.Y, new MatrixTransform(
                           base.DocumentToClientTransform ).Inverse);
  return new System.Drawing.PointF((float)result.X, (float)result.Y);
}

public System.Drawing.PointF DocumentToClient(System.Drawing.PointF point)
{
  Point result = new Point(point.X, point.Y, base.DocumentToClientTransform);
  return new System.Drawing.PointF((float)result.X, (float)result.Y);
}

public System.Drawing.Drawing2D.Matrix DocumentToClientTransformation
{
  get
  {
    return base.DocumentToClientTransform.CreateGdiMatrix();
  }
}
```

Code sample: ClientToDocument and DocumentToClient (see the compatibility sources)



#### CurrentPage

The CurrentPage property has been renamed to CurrentPageIndex, as it holds an index, and not a Page type.



#### DocumentToPage() and PageToDocument()

These transformations are no longer present. For these methods the same applies as for ClientToDocument and DocumentToClient. See below for an example implementation.


```
public System.Drawing.PointF DocumentToPage(
         int pageIndex, System.Drawing.PointF point)
{
  // Note: in 1.0, page coordinates had the origin at the lower left corner.

  Page page = Document.Pages[pageIndex];

  // Pdf page space -> winforms page space -> Document space.
  MatrixTransform transformation = new MatrixTransform(
                                              GetPdfPageTransform(page));
  transformation.Append(GetPageToDocumentTransform(page));

  // Inverse.
  Point result = new Point(point.X, point.Y, transformation.Inverse);
  return new System.Drawing.PointF((float)result.X, (float)(result.Y));
}

public System.Drawing.PointF PageToDocument(
                                  int pageIndex, System.Drawing.PointF point)
{
  // Note: in 1.0, page coordinates had the origin at the lower left corner.

  Page page = Document.Pages[pageIndex];
  // Pdf page space -> winforms page space -> Document space.
  MatrixTransform transformation = new MatrixTransform( 
                                              GetPdfPageTransform(page));
  transformation.Append(GetPageToDocumentTransform(page));

  Point result = new Point(point.X, point.Y, transformation);
  return new System.Drawing.PointF((float)result.X, (float)result.Y);
}
```

Code sample: PageToDocument and DocumentToPage (see compatibility sources).


Also see the remarks on PageRectangles.



#### GetDocumentToPageTransformation() and GetPageToDocumentTransformation()

This is similar to DocumentToPage and PageToDocument. See the compatibility sources for the exact code.



#### GoToFirstPage(), GoToLastPage(

The following code provides the same functionality as GoToFirstPage() and GoToLastPage();


```
pagesViewer.CurrentPageIndex = 0;
pagesViewer.CurrentPageIndex = Document.Pages.Count – 1;
```

Code sample: Implementing GoToFirstPage and GoToLastPage



#### LinkClicked events

The PagesViewer of the standard edition of PDFReaderControls.NET 1 fired three different events when a particular type of link was clicked:
&nbsp;<ul><li>
PageLinkClicked: fired when a link to a page in the same PDF was clicked.</li><li>
OpenFileLinkClicked: fired when a reference to a file was clicked.</li><li>
UrlLinkClicked: fired when an URL was clicked.</li></ul>&nbsp;
These events no longer exist. Instead, one can assign a custom action handler to the PagesViewer which performs the necessary operations.


The LinkClickedHandler sample shows how this can be done. It uses a custom action handler that raises these events itself. Note that the GotoUri and the Launch methods of the DefaultActionHandler class are no-ops. PDFControls will never open a link or file automatically by itself. If you want to implement this, you will have to do so in your own action handler.


Previously, the ActionHandler was only available in the professional edition. The standard edition however, offers support for a limited set of actions only.


```
public class MyActionHandler : DefaultActionHandler
{
  public event PageLinkClickedEventHandler PageLinkClicked = null;
  public event UrlLinkClickedEventHandler UrlLinkClicked = null;
  public event OpenFileLinkClickedEventHandler OpenFileLinkClicked = null;

  public MyActionHandler(PagesViewer pagesViewer ) : base( pagesViewer )
  {
  }

  public override void GoToInternalDestination(
    TallComponents.PDF.Navigation.InternalDestination internalDestination,
    TallComponents.PDF.Actions.ActionContext context)
  {
    if (PageLinkClicked != null)
    {
      PageLinkClicked(this, new PageLinkClickedEventArgs(internalDestination));
    }
    base.GoToInternalDestination(internalDestination, context);
  }

  public override void GoToUri(
    string uri, bool isMap, TallComponents.PDF.Actions.ActionContext context)
  {
    if (UrlLinkClicked != null)
    {
      UrlLinkClicked(this, new UrlLinkClickedEventArgs(uri));
    }
    base.GoToUri(uri, isMap, context);
  }

  public override void Launch(
    string path,
    TallComponents.PDF.Navigation.WindowBehavior windowBehavior, 
    TallComponents.PDF.Actions.ActionContext context)
  {
    if (OpenFileLinkClicked != null)
    {
      OpenFileLinkClicked(this, new OpenFileLinkClickedEventArgs(path));
    }
    base.Launch(path, windowBehavior, context);
  }
}
```

Code sample: Custom Action Handler



#### PageBackColor

The background of a page can be specified in the RenderSetting of the PagesViewer. See RenderSettings.ColorSettings.BackColor.



#### PageRectangles

This property no longer exists. The following code will obtain the rectangle that a page occupies in the coordinate system of its document. If you do this for all pages, you will obtain all “page rectangles”.


```
TallComponents.Interaction.Rectangle pageRectangle =
       new TallComponents.Interaction.Rectangle(0, 0, page.Width, page.Height);
pageRectangle = new TallComponents.Interaction.Rectangle(
       pageRectangle, GetPageToDocumentTransform(page));
System.Drawing.Rectangle rectangle = pageRectangle.CreateGdiRectangle();
```

Code sample: Obtaining page rectangles.



#### PrePaint, PaintReady and PostPaint events

Although the PagesViewer class does no longer provide these events, it is easy to implement them by overriding OnPaint on the PagesViewer.


```
protected override void OnPaint(System.Windows.Forms.PaintEventArgs e)
  {
     if (PrePaint != null)
        PrePaint(this, e);

     base.OnPaint(e);

     if (PostPaint != null)
        PostPaint(this, e);

     if (PaintReady != null  && !Painting) 
        PaintReady(this, new EventArgs());
  }
```

Code sample: Prepaint, postpaint and paintready events.



#### VerticalSpacing

This property no longer exists. Instead, the StandardPagesViewer defines a PageSpacing property, which holds values for Bottom, Top, Left and Right.


The StandardPagesViewer derives these from the spacing of the active layout manager. Currently this is only supported for instances of the GridLayoutManager.



## ThumbnailsViewer

The standard edition of the ThumbnailsViewer has not changed in an incompatible way.



## BookmarksViewer


#### AfterBookmarkSelected

The AfterBookMarkSelected event still exists, but the type of the event arguments has changed:
&nbsp;<ul><li>
The event arguments no longer contain the mouse state. These can easily be obtained via System.Windows.Forms.Control.MouseButtons etc.</li><li>
The referred Bookmark no longer contains a destination property. See the remarks on the Bookmark type below.</li></ul>&nbsp;
Note that it is still possible to cancel the event. The result of cancelling is that the bookmark actions will not be executed subsequently.



#### Bookmark

The bookmark class no longer contains a destination property. When a bookmark is clicked, the PDF specification allows a number of actions to get invoked, which is incompatible with a single destination.


It is still possible to intercept the destinations that PDFReaderControls 1 supported when a user clicks on a bookmark. This can be done via a custom ActionHandler, in the same way as for the PagesViewer class (see the section about LinkClicked events).


Previously, the ActionHandler was only available in the professional edition. The standard edition now also support the ActionHandler, but for a limited set of actions only.



## PrintSettings

The PrintSettings class has been restructured. It now stores a number of settings in the PagePrintSettings class. These are settings that can vary per printed page:
&nbsp;<ul><li>
AutoRotate</li><li>
Color</li><li>
HorizontalAlignment</li><li>
Landscape</li><li>
PaperSize</li><li>
PaperSource</li><li>
RenderSettings</li><li>
ScaleToFit</li><li>
Transform</li><li>
UsePdfPageSize</li><li>
UsePrintableArea</li><li>
VerticalAlignment</li></ul>&nbsp;
The way this works has stayed the same: if one subscribes to the QueryPagePrintSettings event of the PrintSettings, one gets the opportunity to change these settings. These will then come into effect for every next page that will be printed.


The standard edition now also offers access to the RenderSettings. This allows one to influence the quality of the output.



#### Transforms

Previously, the print settings allowed one to specify a Transformation of type System.Drawing.Drawing2D.Matrix. This has been replaced by the Transform property of type TallComponents.PDF.Transforms.Transform. It is possible to create a transform from a matrix via the MatrixTransform constructor.


