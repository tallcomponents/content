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

## Specify dither matrix

A common use of dither is converting a grayscale image to black and white, such that the density of black dots in the 
new image approximates the average gray-level in the original.
Dithering is the processing of applying a matrix to the original image and thereby adding seemingly randomized noise. 
This is done to prevent color banding and other large scale patterns to occur in your image. 
<a href="https://en.wikipedia.org/wiki/Dither">Read more</a>.

![Dither example](/guide/pdfrasterizer4/media/Michelangelo's_David_-_Floyd-Steinberg.png)

The `ConvertToTiffOptions.DitherMatrix` property can be used to specify a dither matrix. 
This is only useful when converting to monochrome image.
The following code converts a PDF page to a bitmap image, while applying a dither matrix to the generated image:

``` csharp
var matrix = new int[][] { 
  new int[] {  1,  9,  3, 11 },
  new int[] { 13,  5, 15,  7 },
  new int[] {  4, 12,  2, 10 },
  new int[] { 16,  8, 14,  6 } 
};

var options = new ConvertToTiffOptions
{
  Resolution = 150f,
  Compression = TiffCompression.CcittG4,
  DitherMatrix = matrix
};

var pdf = new Document(pdfStream);
var page = pdfDocument.Pages[0];

using (var tiffStream = new FileStream("image.tiff", FileMode.Create))
{
    page.ConvertToTiff(tiffStream, options);
}
```