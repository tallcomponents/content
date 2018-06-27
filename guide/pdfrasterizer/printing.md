# Printing

Printing a PDF document comes down to rendering the PDF pages to a System.Drawing.Graphics object that represents a printer. So, basically the process is similar to rendering a PDF document to a raster image as described in Chapter 3. Nonetheless there are some points specif to printing PDF documents. These are discussed in this chapter.



## Basic code sample

Here is the basic code sample of a console application that prints a PDF document to the default printer using default settings.


```
static int pageIndex = 0;
static Document document;

static void Main(string[] args)
{
   using (FileStream file = new FileStream(
      @"document.pdf", FileMode.Open, FileAccess.Read))
   {
      document = new Document(file);

      PrintDocument printDocument = new PrintDocument();
      printDocument.DocumentName = document.DocumentInfo.Title;

      printDocument.PrintPage += new PrintPageEventHandler(printPage);
      printDocument.Print();
   }
}

static void printPage(object sender, PrintPageEventArgs e)
{
   e.Graphics.PageUnit = GraphicsUnit.Point;
   Page page = document.Pages[pageIndex++];
   page.Draw(e.Graphics);
   e.HasMorePages = pageIndex < document.Pages.Count; 
}
```


## Select the output printer

You can only print to printers that have a printer driver installed. The installed printers can be retrieved and selected using the following code:


```
PrinterSettings.StringCollection printerNames =
   PrinterSettings.InstalledPrinters;
// select the first printer
string printerName = printerNames[0];

using (FileStream file = new FileStream(
   @"document.pdf", FileMode.Open, FileAccess.Read))
{
   document = new Document(file);

   PrintDocument printDocument = new PrintDocument();
   printDocument.PrinterSettings.PrinterName = printerName;

   printDocument.DocumentName = document.DocumentInfo.Title;

   printDocument.PrintPage += new PrintPageEventHandler(printPage);
   printDocument.Print();
}
```


## Select the output tray

```
static void Main(string[] args)
{
   using (FileStream file = new FileStream(
      @"document.pdf", FileMode.Open, FileAccess.Read))
   {
      document = new Document(file);

      PrintDocument printDocument = new PrintDocument();

      PrinterSettings.PaperSourceCollection paperSources = 
         printDocument.PrinterSettings.PaperSources;
      printDocument.PrinterSettings.DefaultPageSettings.PaperSource =
         paperSources[0];

      printDocument.DocumentName = document.DocumentInfo.Title;

      printDocument.PrintPage += new PrintPageEventHandler(printPage);
      printDocument.Print();
   }
}

static void printPage(object sender, PrintPageEventArgs e)
{
   e.Graphics.PageUnit = GraphicsUnit.Point;
   Page page = document.Pages[pageIndex++];
   page.Draw(e.Graphics);
   e.HasMorePages = pageIndex< document.Pages.Count; 
}
```


## Scale and rotate to fit

A typical operation is to scale and rotate the source page to fit the output paper. This is done by applying a geometric transformation to the System.Drawing.Graphics object before rendering the PDF page. The following PrintPahge event handler shows how to do this:


```
void printPage( object sender, PrintPageEventArgs e )
{
   e.Graphics.PageUnit = GraphicsUnit.Point;

   Page page = document.Pages[ pageIndex++ ];

   autoPosition( 
      e.Graphics,
      ( e.PageSettings.PaperSize.Width * 72 ) / 100, 
      ( e.PageSettings.PaperSize.Height * 72 ) / 100,
      // As of .NET 2.0, you can use the printable area 
      // instead of the papersize.
      // ( e.PageSettings.PrintableArea.Width * 72 ) / 100, 
      // ( e.PageSettings.PrintableArea.Height * 72 ) / 100,
      page.Width, page.Height );

   page.Draw( e.Graphics )

   e.HasMorePages = pageIndex < document.Pages.Count; 
}

void autoPosition( Graphics graphics, double printerWidth, 
   double printerHeight, double pdfWidth, double pdfHeight )
{
   double scaleX = printerWidth / pdfWidth;
   double scaleY = printerHeight / pdfHeight;
   double scale = Math.Min( scaleX, scaleY );

   graphics.ScaleTransform( (float) scale, (float) scale );
   graphics.TranslateTransform( 
      (float) ( printerWidth - scale * pdfWidth ) / 2, 
      (float) ( printerHeight - scale * pdfHeight ) / 2 );
}
```


## Control font substitution

Font substitution is discussed in detail in <a href="text-and-fonts">Text and Fonts</a>.



## Code samples

The following code samples are included in the distribution and are relevant to printing PDF documents:
&nbsp;<ul><li>
PrintPDF</li><li>
ScaledPrinting</li></ul>&nbsp;
