# Navigation

In addition to the standard navigation means of a PDF reader application, PDF allows you to include navigation elements such as bookmarks and links. Central to navigation is the Destination concept. A Destination encapsulates all information that a reader application needs to jump to a location inside or outside a PDF document.

## Destination

A destination can either be internal or remote. An internal destination is a location inside the current PDF document. A remote destination points to a location inside another PDF document. Finally, a destination can be named. A named destination can be resolved to an internal destination through the Document.NamedDestinations property. This is a collection that maps names to internal destinations. Destination is an abstract base class. InternalDestination, RemoteDestination and NamedDestination are concrete specializations. See class hierarchy below.

![destination-class-hierarchy](/guide/pdfkit/media/destination-class-hierarchy.png)

## Internal destination

The following code snippet adds a link to the first page that points to the third page.


``` csharp
// create the internal destination object:
InternalDestination destination = new InternalDestination();
destination.Page = document.Pages[2];
destination.PageDisplay = PageDisplay.FitEntire;
// add the destination to a link via a GoToAction:
GoToAction action = new GoToAction( destination );
Link link = new Link( left, top, width, height );
link.MouseUpActions.Add( action );
// add the link to the first page:
document.Pages[0].Links.Add( link );
```

Code sample: Add a cross-reference link inside the same document (InternalDest)

## Remote destination

The following code snippet adds a link to the first page that points to the third page of another PDF document. It also instructs the PDF reader to open the document in a new window.


```
// create the remote destination:
RemoteDestination destination = new RemoteDestination(); 
destination.FileSpecification = @"c:\temp\Upgrading to PDFKit.NET 2.0.pdf";
destination.PageIndex = 2; // third page
destination.PageDisplay = PageDisplay.FitEntire;
destination. WindowBehavior = WindowBehavior.NewWindow;
// add the destination to a link via a GoToAction:
GoToAction action = new GoToAction( destination );
Link link = new Link( left, top, width, height );
link.MouseUpActions.Add( action );
// add the link to the first page:
document.Pages[0].Links.Add( link );
```

Code sample: Add a link to an external PDF document (RemoteDest)

## Named destination

The following code snippet dumps all named destination in a given document and reports to what page the associated internal destination points.


```
foreach ( string name in document.NamedDestinations.Names )
{
   // resolve name to internal destination
   InternalDestination destination = document.NamedDestinations[ name ];
   Console.WriteLine( "{0} points to page {1}",
      name, destination.Page.Index );
}
```

Code sample: Dump named destinations

## Links

Links are areas that navigate the PDF reader to another location. In PDF, links are implemented as Link annotations. A link annotation occupies a rectangle on a page and when clicked a sequence of PDF actions is executed. Although this can be any sequence of any actions, it is most often a single GoToAction that points to a given Destination. Code sample **_Add a cross-reference link inside the same document_** and Code sample **_Add a link to an external PDF document_** demonstrate how to create a link and associate it with a destination.

## Bookmarks

A PDF document has a bookmarks tree which is also known as the document outline or table of contents. See image to the right. The top-level bookmarks can be accessed through the Document.Bookmarks property. From here you can enumerate the entire tree. You can also add, insert and remove bookmarks and modify bookmark properties.


The following code splits a document into parts according to its top-level bookmarks. Each part is named according to the title of the bookmark.

![bookmarks](/guide/pdfkit/media/bookmarks.png)

```
BookmarkCollection bookmarks = document.Bookmarks;
for ( int index=0, fromPage=0; index<bookmarks.Count; index++ )
{
   // determine last page of next part
   int toPage = -1;
   Bookmark bookmark = null;
   if ( index == bookmarks.Count-1 )
   {
      // last bookmark - append all remaining pages
      toPage = document.Pages.Count;
   }
   else
   {
      // not the last bookmark - append up to the next bookmark
      bookmark = bookmarks[index+1];
      GoToAction action = bookmark.Actions[0] as GoToAction;
      if ( null != action )
      {
         InternalDestination destination = action.Destination as
            InternalDestination;
         if ( null != destination )
         {
            toPage = destination.Page.Index;
         }
      }
   }

   // create a new part
   if ( -1 != toPage && null != bookmark )
   {
      Document part = new Document();
      for ( int pageIndex=fromPage; pageIndex<toPage; pageIndex++ )
      {
         part.Pages.Add( document.Pages[pageIndex].Clone() );
      }
      using ( FileStream fileOut = new FileStream( 
         bookmark.Title + ".pdf", FileMode.Create, FileAccess.Write ) )
      {
         part.Write( fileOut );
      }
      fromPage = toPage;
   }
}
```

Code sample: Split a document according to its top-level bookmarks (SplitByBookmark)

## Viewer Preferences

The viewer preferences define how a PDF reader should display a PDF document initially. The following code snippet shows you how to open, change and save a PDF so that the next time you open it, it will be displayed without menu and toolbar.

```
// open file 
using ( FileStream fileIn = new FileStream( 
  "guide.pdf",
  FileMode.Open, FileAccess.Read ) )
{
  // open source PDF
  Document document = new Document( fileIn );

  // assign custom viewer preferences
  ViewerPreferences preferences = new ViewerPreferences();
  preferences.HideMenubar = true;
  preferences.HideToolbar = true;
  document.ViewerPreferences = preferences;

  // save as a new PDF document
  using ( FileStream fileOut = new FileStream( 
    "guide_nomenunotoolbar.pdf", 
    FileMode.Create, FileAccess.Write ) )
  {
    document.Write( fileOut );
  }
}
```

Code sample: Save PDF document with modified viewer preferences (ViewerPrefs)


