# Convert PDF to WBF XPS

**Note** This chapter only applies to the .NET 3.0 build.


In addtion to rendering PDF pages to raster images such as BMP, PNG and JPG, PDFRasterizer,NET 3.o also lets you convert PDF documents to WPF graphics or XPS documents. All vector graphics are preserved. This is new as of version 3.0.



## Save PDF as XPS

Central to the conversion to WPF/XPS are the Document.ConvertToWpf and Page.ConvertToWpf methods. These methods create WPF FixedDocument and WPF FixedPage instances, respectively.


The following basic code sample demonstrates how to create a FixedDocument from a PDF document:


```
[STAThread]
static void Main(string[] args)
{
   using (FileStream pdfFile = new FileStream(
      @"document.pdf", FileMode.Open, FileAccess.Read))
   {
      RenderSettings renderSettings = new RenderSettings();
      ConvertToWpfOptions options = new ConvertToWpfOptions();

      Document document = new Document(pdfFile);
      FixedDocument fixedDocument = document.ConvertToWpf(
         renderSettings, options);

      XpsDocument xpsDocument= new XpsDocument(
         @"document.xps", FileAccess.ReadWrite);
      XpsDocumentWriter xpsWriter =
         XpsDocument.CreateXpsDocumentWriter(xpsDocument);
      xpsWriter.Write(fixedDocument);
   }
}
```

Code sample: Save a PDF document as XPS


Note that the calling threat must live in an STA. Unfortunately COM still haunts us today.



## Display PDF pages as WPF graphics

This is demonstrated by code sample ViewPDF_WPF which is included in the product download.



## Code samples

The following code samples are included in the distribution and are relevant to converting PDF documents to WPF/XPS:
&nbsp;<ul><li>
ConvertToXPS</li><li>
ViewPDF_WPF</li></ul>&nbsp;
