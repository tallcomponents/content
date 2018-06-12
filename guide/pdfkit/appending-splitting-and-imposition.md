# Appending, Splitting and Imposition

Using this application it is possible to construct new documents from pages from existing documents. This way you can split existing documents into multiple new document, you assemble new documents from multiple existing documents and you can create new pages that are composed of multiple new or existing pages.

## Append

Appending pages to a document is extremely simple. The following code shows how a new output document is created from an assortment of new and existing pages. Note that 3 different methods for adding a range of pages are shown.

```
using ( FileStream file1 = new FileStream( 
   @"..\..\..\f1040a.pdf", FileMode.Open, FileAccess.Read ) )
using ( FileStream file2 = new FileStream( 
   @"..\..\..\fw4.pdf", FileMode.Open, FileAccess.Read ) )
using ( FileStream file3 = new FileStream( 
   @"..\..\..\ResellerGuide.pdf", FileMode.Open, FileAccess.Read ) )
{
   Document document = new Document();

   // add all pages from f1040a.pdf (using foreach)
   Document document1 = new Document( file1 );
   foreach ( Page page in document1.Pages )
   {
      document.Pages.Add( page.Clone() );
   }

   // add all pages from fw4.pdf (using AddRange)
   Document document2 = new Document( file2 );
   document.Pages.AddRange( document2.Pages.CloneToArray() );

   // add all pages from ResellerGuide.pdf (using page index)
   Document document3 = new Document( file3 );
   for ( int i=0; i<document3.Pages.Count; i++ )
   {
      document.Pages.Add( document3.Pages[i].Clone() );
   }

   // save new document with all pages
   using ( FileStream file = new FileStream( 
      @"..\..\out.pdf", FileMode.Create, FileAccess.Write ) )
   {
      document.Write( file );
   }
}
```

**Code sample: Combine all pages of multiple documents in one document (Append)**

## Split

Splitting a document into multiple documents really boils down to the same kind of code that you use to append pages to a document. The following code shows how a document is split in chunks of 5 or less pages:

```
using ( FileStream fileIn = new FileStream( 
   @"ResellerGuide.pdf", FileMode.Open, FileAccess.Read ) )
{
   // open source document
   Document documentIn = new Document( fileIn );
   int n = documentIn.Pages.Count;
   for ( int i=0; i<n; i+=5 )
   {
      // create a page array of the next 5 (or less) pages
      Page[] pages = new Page[ Math.Min( i+5, n ) - i ];
      for ( int j=0; j<pages.Length; j++ )
      {
         pages[j] = documentIn.Pages[i+j].Clone();
      }
      // create a new document and add the range of pages
      Document document = new Document();
      document.Pages.AddRange( pages );
      using ( FileStream fileOut = new FileStream( 
         string.Format( @"..\..\out{0}-{1}.pdf", i+1, i+pages.Length ),
         FileMode.Create, FileAccess.Write ) )
      {
         // save next document
         document.Write( fileOut );
      }
   }
}
```

**Code sample: Split a document in chunks of 5 or less pages (Split)**

## Imposition

Figure below shows a typical imposition scenario known as 2-up.

![Typical-imposition-scenario-2-up](guide/pdfkit/media/Typical-imposition-scenario-2-up.png)

**Typical imposition scenario 2-up**

The following code sample shows how to implement the 2-up scenario.

```
using ( FileStream fileIn = new FileStream( 
   "ResellerGuide.pdf", FileMode.Open, FileAccess.Read ) )
{
   // open source document
   Document documentIn = new Document( fileIn );
   PageCollection pages = documentIn.Pages;

   // set new width and height
   double height = pages[0].Width;
   double width = pages[0].Height;

   // create new document
   Document document = new Document();
   for ( int i=0; i<pages.Count; i+=2 )
   {
      Page page = new Page( width, height );    

      // add left page as page shape
      PageShape pageShape1 = new PageShape( 
         pages[i], 0, 0, width / 2, height, true, 0,
         PageBoundary.MediaBox );
      page.Overlay.Add( pageShape1 );

      if ( i+1 == pages.Count ) break;

      // add right page as page shape
      PageShape pageShape2 = new PageShape(
         pages[i+1], width / 2, 0, width / 2, height, true, 0,
         PageBoundary.MediaBox );
      page.Overlay.Add( pageShape2 );

      document.Pages.Add( page );
   }

   // save new document
   using ( FileStream fileOut = new FileStream( 
      "out.pdf", FileMode.Create, FileAccess.Write ) )
   {
      document.Write( fileOut );
   }
}
```

**Code sample: Create 2-up document (2up)**


