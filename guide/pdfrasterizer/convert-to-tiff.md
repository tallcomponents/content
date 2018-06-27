# Convert to TIFF

Coversion of PDF to (multipage) TIFF is a typical usage of PDFRasterizer.NET. Because GDI+ is somewhat restricted we offer a dedicated conversion to single and multi-page TIFF.



## Basic code sample

The following basic code sample converts a PDF document to a multi-page TIFF document.


```
using (FileStream pdfFile = new FileStream(
   @"document.pdf", FileMode.Open, FileAccess.Read))
using (FileStream tiffFile = new FileStream(
   @"document.tiff", FileMode.Create, FileAccess.Write))
{
   ConvertToTiffOptions options = new ConvertToTiffOptions();
   options.Compression = TiffCompression.CcittG3;
   options.Resolution = 300;

   Document document = new Document(pdfFile);
   document.ConvertToTiff(tiffFile, options);
}
```

Code sample: Convert a PDF document to a multipage TIFF document


A similar ConvertToTiff method exists on the Page class. This results in a single-page TIFF document.



## B&W and Color

You can convert either to B&W or to color by setting the ConvertToTiffOptions.PixelFormat property. Set this property to Bw1Bpp to render to a B&W TIFF. Set this property to Rgba32Bpp to render to a color TIFF .



## Compression

Per pixel format, a number of compressions are available. You select the compression by settings the ConvertToTiffOptions.Compression property. The following table shows the possible combinations:
&nbsp;<table><tr><th>
PixelFormat</th><th>
Compression</th></tr><tr><td>
Bw1Bpp</td><td>
None


CcittG3


CcittG4


PackBits


Lzw</td></tr><tr><td>
Bw1Bpp</td><td>
None


Lzw</td></tr></table>&nbsp;
