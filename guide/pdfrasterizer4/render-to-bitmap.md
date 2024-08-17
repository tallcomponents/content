# Render to bitmap

The following code saves the first page of a PDF document as a PNG at 150 DPI:

``` csharp
using (var pdfFile = new FileStream(pdfPath, FileMode.Open, FileAccess.Read))
using (var imageFile = new FileStream(imagePath, FileMode.Create))
{
  var pdf = new Document(pdfFile);
  var page = pdf.Pages[0];
  float resolution = 150f; // 150 DPI
  page.SaveAsBitmap(imageFile, ImageEncoding.Png, resolution);
}
```

## Image formats

The following image formats are supported:

- ImageEncoding.Png
- ImageEncoding.Jpeg