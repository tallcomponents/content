# Render to Skia Surface

Starting with version 4.0, we use the [Skia 2D graphics library](https://skia.org) to render PDF pages. 
Methods `Page.Save` and `Document.ConvertToTiff` can be used to render pages or documents directly to single- or multipage raster images.
It is however also possible to render to a Skia surface. This is demonstrated by the following code samples.

## Open PDF and select the first page

``` csharp
var pdf = new Document(new FileStream(path, FileMode.Open, FileAccess.Read));
var page = pdf.Pages[0];
```

## Calculate the size of the image

``` csharp
const float cScale = 1.0f; // 72 dpi
var pageBox = page.CropBox ?? page.MediaBox;
int width = (int)Math.Round(pageBox.Width * cScale);
int height = (int)Math.Round(pageBox.Height * cScale);
if (pdfPage.Rotation % 180 != 0)
{
  int tmp = width;
  width = height;
  height = tmp;
}
```

## Render page into Skia surface

``` csharp
using (var surface = SKSurface.Create(info))
{
  page.Draw(surface, info, cScale);
  // ...
}
```

## Save rendering buffer to raster image 

``` csharp
using (var pixmap = surface.PeekPixels())
using (var stream = new SKFileWStream(imgFile))
{
  pixmap.Encode(stream, SKEncodedImageFormat.Jpeg, 90);
  stream.Flush();
}
```
