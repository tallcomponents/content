# Convert to TIFF

Conversion of PDF to single- or multipage TIFF is a common use. Therefor we offer a conveniency method to do this easily.

## Convert to black and white

The following code converts a PDF document to a multi-page 1bpp TIFF document.

``` csharp
using (var pdfFile = new FileStream(pdfPath, FileMode.Open, FileAccess.Read))
using (var tiffFile = new FileStream(tiffPath, FileMode.Create))
{
  var pdf = new Document();
  var options = new ConvertToTiffOptions
  {
    Resolution = 150f,
    Compression = TiffCompression.CcittG4
  };
  pdf.ConvertToTiff(file, tiffOptions);
}
```

A similar ConvertToTiff method exists on the Page class. This results in a single-page TIFF document.

## Convert to color

The following code converts a PDF document to a multi-page 8bpc RGBA TIFF document.

``` csharp
using (var pdfFile = new FileStream(pdfPath, FileMode.Open, FileAccess.Read))
using (var tiffFile = new FileStream(tiffPath, FileMode.Create))
{
  var pdf = new Document();
  var options = new ConvertToTiffOptions
  {
    Resolution = 150f,
    Compression = TiffCompression.Lzw,
    PixelFormat = PixelFormat.Rgba32Bpp
  };
  pdf.ConvertToTiff(file, tiffOptions);
}
```
